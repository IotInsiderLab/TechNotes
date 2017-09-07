# ESP32

## Device Updates
 You will likely want a dedicated cpp file for handling the update logic

```C++
#include "Arduino.h"

extern "C"
{
    #include "esp_request.h"    
}

// If read and write are defined then the compiler
// will mangle the methods in the header and in this
// file so undo any definition you may already have
#undef read
#undef write

#include "Update.h"

// Think of the file as a singleton object
// and these are its member variables.

static String url;
static String md5;

static int buffer_len;
static int content_len;

int updater_cb( request_t* req, uint8_t* data, int len )
{
    // if the http status code is in the success range
    if( req->response->status_code >= 200 && req->response->status_code < 300 )
    {
        // if this is the first block then try to
        // get the content length from the header
        if( content_len == 0 )
        {
            bool ready = false;
            req_list_t *found = req->response->header;
            found = req_list_get_key( req->response->header, "Content-Length" );
            if( found )
            {
                int size = atoi( ( char* ) found->value );
                //TODO: Log content length found
                ready = Update.begin( size );
            }
            else
            {
                // We don't strictly need the content
                // length, it's just more efficient
                ready = Update.begin();
            }
        }
        buffer_len += len;
        Update.write( data, len );
    }
    else
    {
        //TODO: log status_code, content_len, buffer_len, len
        // sending an error message to the cloud here might not
        // make sense since this is likely a network error
    }
    return 0;
}

static void request_task( void *pvParameters )
{
    int download_status = -1;
    bool md5_set = Update.setMD5( md5.c_str() );

    // if md5 has the correct format, process the request
    if( md5_set )
    {
        // md5 set correctly, download image
        request_t *req;
        req = req_new( url.c_str() );
        req_setopt( req, REQ_FUNC_DOWNLOAD_CB, ( void* )updater_cb );
        download_status = req_perform( req );
        req_clean( req );
    }
    else
    {
        // md5 not set
        Update.abort();
    }

    bool update_status = Update.end();

    //TODO: log and send message to cloud with
    // download_status and update_status then
    // wait a few seconds for message to complete
    // sending
    vTaskDelay( 2000 );
    ESP.restart();

    vTaskDelete( NULL );
}

// ota_update
//
// This is the function that gets called from the Cloud to Device
// message handler. It needs a url to the image file and the md5
// hash
void ota_update( char* ota_url, char* ota_md5 )
{
    // Initialize state
    url = ota_url;
    md5 = ota_md5;

    buffer_len = 0;
    content_len = 0;

    // Queue Download/Update task
    xTaskCreate( &request_task, "request_task", 8192, NULL, 5, NULL );
}
```
