# Cognitive Services - Vision
Helpful notes on using Cognitive Services - Vision API.

**Note:** _use the azure portal to generate keys_

## Computer Vision

### Analyze Image

Calls multiple functions at once (gets spendy)

#### visualFeatures
- Categories - categorizes image content according to a taxonomy defined in documentation.
- Tags - tags the image with a detailed list of words related to the image content.
- Description - describes the image content with a complete English sentence.
- Faces - detects if faces are present. If present, generate coordinates, gender and age.
- ImageType - detects if image is clipart or a line drawing.
- Color - determines the accent color, dominant color, and whether an image is black&white.
- Adult - detects if the image is pornographic in nature (depicts nudity or a sex act). Sexually suggestive content is also detected.

#### details

- Celebrities - identifies celebrities if detected in the image.
- Landmarks - identifies landmarks if detected in the image.

```bash
$ curl -v -X POST "https://westus.api.cognitive.microsoft.com/vision/v1.0/analyze?visualFeatures=Categories,Tags,Description,Faces,ImageType,Color,Adult&details=Celebrities,Landmarks&language=en" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" -F "image=@image.jpg" >image.json
```

```json
{
  "categories":[
    {
      "name":"people_group",
      "score":0.796875,
      "detail":{
        "celebrities":[
          {
            "faceRectangle":{
              "top":991, "left":1417, "width":201, "height":201 },
            "name":"Dan Patrick",
            "confidence":0.91310310363769531
          }
        ]
      }
    }
  ],
  "tags":[
    { "name":"person",   "confidence":0.99883431196212769 },
    { "name":"posing",   "confidence":0.95219212770462036 },
    { "name":"standing", "confidence":0.9176596999168396 },
    { "name":"people",   "confidence":0.76557600498199463 },
    { "name":"group",    "confidence":0.75815021991729736 }
  ],
  "description":{
    "tags":[
      "person","posing","standing","photo","people","group",
      "man","holding","woman","front","young","brown","friends",
      "room","living","bunch","large","wine","suit","white"
    ],
    "captions":[
      {
        "text":"Dan Patrick et al. posing for a photo",
        "confidence":0.98480619590853158
      }
    ]
  },
  "faces":[
    {
      "age":30,
      "gender":"Male",
      "faceRectangle":{
        "top":774, "left":713, "width":211, "height":211 }
    },
    {
      "age":59,
      "gender":"Male",
      "faceRectangle":{
        "top":993, "left":1421, "width":204, "height":204 }
    },
    {
      "age":54,
      "gender":"Male",
      "faceRectangle":{
        "top":977, "left":1918, "width":182, "height":182 }
    },
    {
      "age":46,
      "gender":"Female",
      "faceRectangle":{
        "top":1056, "left":2279, "width":218, "height":218 }
    },
    {
      "age":46,
      "gender":"Male",
      "faceRectangle":{
        "top":817, "left":2729, "width":229, "height":229 }
    },
    {
      "age":30,
      "gender":"Female",
      "faceRectangle":{
        "top":1193, "left":2874, "width":259, "height":259 }
    }
  ],
  "adult":{
    "isAdultContent":false,
    "adultScore":0.032763652503490448,
    "isRacyContent":false,
    "racyScore":0.028989236801862717
  },
  "color":{
    "dominantColorForeground":"Black",
    "dominantColorBackground":"Black",
    "dominantColors":["Black","Grey"],
    "accentColor":"897542",
    "isBwImg":false
  },
  "imageType":{
    "clipArtType":0,
    "lineDrawingType":0
  },
  "requestId":"7505fbd9-769b-44f8-93fd-dfa129bfbce5",
  "metadata":{
    "height":3024, "width":4032, "format":"Jpeg" }
}
```

### Describe Image

Generates a description of an image in human readable language with complete sentences

```bash
$ curl -v -X POST "https://westus.api.cognitive.microsoft.com/vision/v1.0/describe?maxCandidates=5" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" -F "image=@image.jpg" >image-describe.json
```

```json

{
  "description": {
    "tags": [
      "outdoor", "road", "street", "grass", "tree", "house", "flower", "front",
      "pink", "sidewalk", "small", "red", "sitting", "park", "hydrant", "bench",
      "side", "little", "garden", "green", "woman", "fire", "parked", "man",
      "riding", "sign", "bushes", "girl", "young", "standing", "large",
      "walking", "parking", "city", "motorcycle", "people"
    ],
    "captions": [
      {
        "text": "a tree with pink flowers on a sidewalk",
        "confidence": 0.94644848608356347
      },
      {
        "text": "a tree with pink flowers in a garden",
        "confidence": 0.94544848608356347
      },
      {
        "text": "a tree with pink flowers in front of a house",
        "confidence": 0.93959136808027544
      },
      {
        "text": "a tree with pink flowers in a parking lot",
        "confidence": 0.916068572058324
      },
      {
        "text": "a tree with pink flowers",
        "confidence": 0.915068572058324
      }
    ]
  },
  "requestId": "b671b736-2d92-4ead-852d-13137d8b3025",
  "metadata": {
    "height": 2255, "width": 4008, "format": "Jpeg"
  }
}
```

### Recognize Domain Specific Content

Recognizes content within an image by applying a domain-specific model.

```bash
$ curl -v -X POST "https://westus.api.cognitive.microsoft.com/vision/v1.0/describe?maxCandidates=5" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" -F "image=@image.jpg" >image-describe.json

# List doamin models available
$ curl -v "https://westus.api.cognitive.microsoft.com/vision/v1.0/models" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" >models.json

# Celebrites
$ curl -v -X POST "https://westus.api.cognitive.microsoft.com/vision/v1.0/models/celebrities/analyze" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" -F "image=@image.jpg" >image-celebrity.json

# Landmarks
$ curl -v -X POST "https://westus.api.cognitive.microsoft.com/vision/v1.0/models/landmarks/analyze" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" -F "image=@image.jpg" >image-landmarks.json
```

```json
{
  "result": {
    "landmarks": [
      {
        "name": "Petra",
        "confidence": 0.99999988079071045
      }
    ]
  },
  "requestId": "a027c884-2c5d-4942-b24a-47a1eb1d8c68",
  "metadata": {
    "height": 742,
    "width": 1200,
    "format": "Jpeg"
  }
}
```

### Tag Image

Generates a list of words, or tags, that are relevant to the content of the supplied image.

```bash
$ curl -v -X POST "https://westus.api.cognitive.microsoft.com/vision/v1.0/tag" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" -F "image=@image.jpg" >image-tag.json
```

```json
{
  "tags": [
    {
      "name": "cat",
      "confidence": 0.99999785423278809
    },
    {
      "name": "white",
      "confidence": 0.99470525979995728
    },
    {
      "name": "sitting",
      "confidence": 0.973540723323822
    },
    {
      "name": "mammal",
      "confidence": 0.91753280162811279,
      "hint": "animal"
    },
    {
      "name": "lagomorph",
      "confidence": 0.91299694776535034,
      "hint": "animal"
    },
    {
      "name": "animal",
      "confidence": 0.9113161563873291
    },
    {
      "name": "indoor",
      "confidence": 0.90246623754501343
    },
    {
      "name": "laying",
      "confidence": 0.74194872379302979
    }
  ],
  "requestId": "82ad448e-cabc-4db0-a67b-9bdd1adb505b",
  "metadata": {
    "height": 829,
    "width": 1024,
    "format": "Jpeg"
  }
}
```

### Get Tumbnail

Generates a thumbnail image with the user-specified width and height by analyzing the image to identify the region of interest.

```bash
$ curl -v -X POST "https://westus.api.cognitive.microsoft.com/vision/v1.0/generateThumbnail?width=500&height=100&smartCropping=true" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" -F "image=@image.jpg" >image-thumb.jpg

```

### OCR

Detects text in an image and extracts the recognized characters into a machine-usable character stream.

```bash
$ curl -v -X POST "https://westus.api.cognitive.microsoft.com/vision/v1.0/ocr?language=unk&detectOrientation=true" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" -F "image=@image.jpg" >image-ocr.json
```

```json
{
  "language": "en",
  "orientation": "Up",
  "textAngle": -0.026179938779914324,
  "regions": [
    {
      "boundingBox": "1616,1088,918,355",
      "lines": [
        {
          "boundingBox": "2476,1088,58,14",
          "words": [
            {
              "boundingBox": "2476,1088,58,14",
              "text": "Pacific"
            }
          ]
        },
        {
          "boundingBox": "1622,1116,577,52",
          "words": [
            {
              "boundingBox": "1622,1135,131,33",
              "text": "Moving"
            },
            {
              "boundingBox": "1770,1126,151,33",
              "text": "Forward"
            },
            {
              "boundingBox": "1936,1123,75,29",
              "text": "with"
            },
            {
              "boundingBox": "2029,1116,170,35",
              "text": "Firewalld"
            }
          ]
        },
        {
          "boundingBox": "1990,1223,295,30",
          "words": [
            {
              "boundingBox": "1990,1228,115,25",
              "text": "Contact"
            },
            {
              "boundingBox": "2116,1223,169,27",
              "text": "Information"
            }
          ]
        },
        {
          "boundingBox": "1617,1323,197,33",
          "words": [
            {
              "boundingBox": "1617,1328,78,28",
              "text": "Cathy"
            },
            {
              "boundingBox": "1705,1326,21,23",
              "text": "L."
            },
            {
              "boundingBox": "1739,1323,75,25",
              "text": "Smith"
            }
          ]
        },
        {
          "boundingBox": "1617,1366,264,31",
          "words": [
            {
              "boundingBox": "1617,1370,92,27",
              "text": "CISSP,"
            },
            {
              "boundingBox": "1721,1368,85,26",
              "text": "CSFA,"
            },
            {
              "boundingBox": "1818,1366,63,24",
              "text": "CEH"
            }
          ]
        },
        {
          "boundingBox": "1616,1411,298,32",
          "words": [
            {
              "boundingBox": "1616,1411,298,32",
              "text": "cathy.smith@pnnl.gov"
            }
          ]
        }
      ]
    },
    {
      "boundingBox": "2874,950,236,291",
      "lines": [
        {
          "boundingBox": "2874,950,236,30",
          "words": [
            {
              "boundingBox": "2874,956,49,24",
              "text": "End"
            },
            {
              "boundingBox": "2933,955,23,22",
              "text": "of"
            },
            {
              "boundingBox": "2968,952,61,23",
              "text": "slide"
            },
            {
              "boundingBox": "3039,950,71,22",
              "text": "show"
            }
          ]
        },
        {
          "boundingBox": "2883,1218,199,23",
          "words": [
            {
              "boundingBox": "2883,1221,52,20",
              "text": "Click"
            },
            {
              "boundingBox": "2943,1221,20,17",
              "text": "to"
            },
            {
              "boundingBox": "2971,1218,41,20",
              "text": "add"
            },
            {
              "boundingBox": "3020,1218,62,18",
              "text": "notes"
            }
          ]
        }
      ]
    }
  ]
}
```

### Recognize Handwritten Text

Detects hand written text in an image and extracts the recognized characters into a machine-usable character stream.

```bash
$ curl -v -X POST "https://westus.api.cognitive.microsoft.com/vision/v1.0/recognizeText?handwriting=true" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" -H "Content-Type:application/octet-stream" --data-binary @image.jpg --dump-header image-recognize-request.txt
$ curl -v -X GET "https://westus.api.cognitive.microsoft.com/vision/v1.0/textOperations/58bee71f-48fd-4906-b7f3-d6ccdf8a004f" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" > image-recognize-result.json
```

```json
{
  "status": "Succeeded",
  "recognitionResult": {
    "lines": [
      {
        "boundingBox": [
          854,
          226,
          1580,
          208,
          1582,
          344,
          858,
          360
        ],
        "text": "Map Z Dfusion",
        "words": [
          {
            "boundingBox": [
              988,
              226,
              1227,
              213,
              1240,
              352,
              1001,
              366
            ],
            "text": "Map"
          },
          {
            "boundingBox": [
              1181,
              215,
              1273,
              210,
              1286,
              349,
              1194,
              355
            ],
            "text": "Z"
          },
          {
            "boundingBox": [
              1245,
              211,
              1585,
              192,
              1598,
              331,
              1259,
              351
            ],
            "text": "Dfusion"
          }
        ]
      },
      {
        "boundingBox": [
          2128,
          260,
          2418,
          108,
          2458,
          182,
          2166,
          334
        ],
        "text": "Seats Perp",
        "words": [
          {
            "boundingBox": [
              2102,
              267,
              2294,
              175,
              2329,
              255,
              2138,
              347
            ],
            "text": "Seats"
          },
          {
            "boundingBox": [
              2287,
              178,
              2424,
              112,
              2459,
              193,
              2322,
              259
            ],
            "text": "Perp"
          }
        ]
      },
      {
        "boundingBox": [
          2694,
          122,
          3192,
          90,
          3200,
          240,
          2704,
          272
        ],
        "text": "UserDB",
        "words": [
          {
            "boundingBox": [
              2606,
              130,
              3200,
              89,
              3200,
              242,
              2595,
              283
            ],
            "text": "UserDB"
          }
        ]
      },
      {
        "boundingBox": [
          848,
          402,
          1802,
          366,
          1804,
          454,
          852,
          490
        ],
        "text": "i Send Charlie @ Sunculture",
        "words": [
          {
            "boundingBox": [
              825,
              406,
              901,
              402,
              889,
              499,
              813,
              503
            ],
            "text": "i"
          },
          {
            "boundingBox": [
              992,
              397,
              1220,
              385,
              1208,
              482,
              980,
              494
            ],
            "text": "Send"
          },
          {
            "boundingBox": [
              1212,
              385,
              1462,
              372,
              1451,
              468,
              1200,
              482
            ],
            "text": "Charlie"
          },
          {
            "boundingBox": [
              1447,
              372,
              1523,
              368,
              1511,
              465,
              1435,
              469
            ],
            "text": "@"
          },
          {
            "boundingBox": [
              1515,
              369,
              1834,
              351,
              1823,
              448,
              1504,
              466
            ],
            "text": "Sunculture"
          }
        ]
      },
      {
        "boundingBox": [
          2100,
          382,
          2236,
          346,
          2252,
          406,
          2116,
          444
        ],
        "text": "LOK",
        "words": [
          {
            "boundingBox": [
              2059,
              383,
              2236,
              349,
              2258,
              414,
              2082,
              448
            ],
            "text": "LOK"
          }
        ]
      },
      {
        "boundingBox": [
          1052,
          512,
          1682,
          482,
          1686,
          580,
          1056,
          610
        ],
        "text": "the do bug option",
        "words": [
          {
            "boundingBox": [
              1028,
              516,
              1171,
              507,
              1182,
              612,
              1038,
              621
            ],
            "text": "the"
          },
          {
            "boundingBox": [
              1187,
              506,
              1291,
              499,
              1302,
              605,
              1198,
              611
            ],
            "text": "do"
          },
          {
            "boundingBox": [
              1259,
              501,
              1427,
              490,
              1437,
              596,
              1270,
              607
            ],
            "text": "bug"
          },
          {
            "boundingBox": [
              1435,
              490,
              1690,
              473,
              1701,
              579,
              1445,
              595
            ],
            "text": "option"
          }
        ]
      },
      {
        "boundingBox": [
          2074,
          472,
          3146,
          240,
          3186,
          424,
          2114,
          656
        ],
        "text": "Celebrity ]",
        "words": [
          {
            "boundingBox": [
              2604,
              352,
              3067,
              268,
              3123,
              461,
              2660,
              545
            ],
            "text": "Celebrity"
          },
          {
            "boundingBox": [
              3083,
              265,
              3166,
              250,
              3200,
              443,
              3140,
              458
            ],
            "text": "]"
          }
        ]
      },
      {
        "boundingBox": [
          1074,
          612,
          1502,
          594,
          1506,
          664,
          1078,
          682
        ],
        "text": "Screenshot",
        "words": [
          {
            "boundingBox": [
              1058,
              616,
              1499,
              589,
              1501,
              665,
              1060,
              692
            ],
            "text": "Screenshot"
          }
        ]
      },
      {
        "boundingBox": [
          2746,
          560,
          3084,
          572,
          3080,
          704,
          2742,
          692
        ],
        "text": "Proty",
        "words": [
          {
            "boundingBox": [
              2674,
              577,
              3079,
              563,
              3098,
              697,
              2692,
              711
            ],
            "text": "Proty"
          }
        ]
      },
      {
        "boundingBox": [
          1454,
          734,
          1656,
          748,
          1650,
          848,
          1446,
          834
        ],
        "text": "450",
        "words": [
          {
            "boundingBox": [
              1407,
              723,
              1673,
              761,
              1648,
              860,
              1382,
              822
            ],
            "text": "450"
          }
        ]
      },
      {
        "boundingBox": [
          2700,
          764,
          3132,
          712,
          3146,
          828,
          2716,
          882
        ],
        "text": "NEF TO",
        "words": [
          {
            "boundingBox": [
              2648,
              785,
              2925,
              739,
              2995,
              845,
              2718,
              891
            ],
            "text": "NEF"
          },
          {
            "boundingBox": [
              3000,
              726,
              3113,
              708,
              3183,
              814,
              3070,
              832
            ],
            "text": "TO"
          }
        ]
      },
      {
        "boundingBox": [
          1558,
          976,
          1672,
          1080,
          1606,
          1152,
          1492,
          1048
        ],
        "text": "72",
        "words": [
          {
            "boundingBox": [
              1485,
              1005,
              1685,
              979,
              1676,
              1089,
              1477,
              1115
            ],
            "text": "72"
          }
        ]
      },
      {
        "boundingBox": [
          1862,
          942,
          2514,
          886,
          2524,
          1012,
          1874,
          1068
        ],
        "text": "40 * 6 ( 6) + 6 ( )",
        "words": [
          {
            "boundingBox": [
              1837,
              931,
              2049,
              920,
              2011,
              1062,
              1798,
              1073
            ],
            "text": "40"
          },
          {
            "boundingBox": [
              2027,
              921,
              2139,
              916,
              2100,
              1058,
              1989,
              1063
            ],
            "text": "*"
          },
          {
            "boundingBox": [
              2083,
              919,
              2195,
              913,
              2156,
              1055,
              2044,
              1061
            ],
            "text": "6"
          },
          {
            "boundingBox": [
              2150,
              915,
              2262,
              910,
              2223,
              1052,
              2112,
              1057
            ],
            "text": "("
          },
          {
            "boundingBox": [
              2172,
              914,
              2318,
              907,
              2279,
              1049,
              2134,
              1056
            ],
            "text": "6)"
          },
          {
            "boundingBox": [
              2296,
              908,
              2407,
              903,
              2369,
              1045,
              2257,
              1050
            ],
            "text": "+"
          },
          {
            "boundingBox": [
              2351,
              905,
              2463,
              900,
              2425,
              1042,
              2313,
              1047
            ],
            "text": "6"
          },
          {
            "boundingBox": [
              2407,
              903,
              2519,
              897,
              2481,
              1039,
              2369,
              1045
            ],
            "text": "("
          },
          {
            "boundingBox": [
              2441,
              901,
              2553,
              895,
              2514,
              1037,
              2402,
              1043
            ],
            "text": ")"
          }
        ]
      },
      {
        "boundingBox": [
          2734,
          922,
          3180,
          910,
          3182,
          1018,
          2738,
          1030
        ],
        "text": "FF Proto",
        "words": [
          {
            "boundingBox": [
              2706,
              945,
              2855,
              931,
              2868,
              1028,
              2718,
              1042
            ],
            "text": "FF"
          },
          {
            "boundingBox": [
              2945,
              922,
              3160,
              902,
              3172,
              999,
              2957,
              1019
            ],
            "text": "Proto"
          }
        ]
      },
      {
        "boundingBox": [
          2000,
          1156,
          2444,
          1136,
          2448,
          1226,
          2004,
          1244
        ],
        "text": "86 = 6 - 128",
        "words": [
          {
            "boundingBox": [
              1978,
              1158,
              2168,
              1149,
              2121,
              1240,
              1931,
              1248
            ],
            "text": "86"
          },
          {
            "boundingBox": [
              2146,
              1150,
              2258,
              1146,
              2211,
              1236,
              2099,
              1241
            ],
            "text": "="
          },
          {
            "boundingBox": [
              2213,
              1148,
              2325,
              1143,
              2278,
              1233,
              2166,
              1238
            ],
            "text": "6"
          },
          {
            "boundingBox": [
              2280,
              1145,
              2392,
              1140,
              2345,
              1230,
              2233,
              1235
            ],
            "text": "-"
          },
          {
            "boundingBox": [
              2314,
              1143,
              2504,
              1135,
              2457,
              1225,
              2267,
              1233
            ],
            "text": "128"
          }
        ]
      },
      {
        "boundingBox": [
          2736,
          1128,
          2926,
          1134,
          2924,
          1200,
          2734,
          1194
        ],
        "text": "Live",
        "words": [
          {
            "boundingBox": [
              2703,
              1131,
              2941,
              1128,
              2944,
              1202,
              2706,
              1205
            ],
            "text": "Live"
          }
        ]
      },
      {
        "boundingBox": [
          280,
          1542,
          974,
          1130,
          1058,
          1272,
          366,
          1684
        ],
        "text": "deployment user",
        "words": [
          {
            "boundingBox": [
              283,
              1544,
              776,
              1254,
              861,
              1397,
              367,
              1687
            ],
            "text": "deployment"
          },
          {
            "boundingBox": [
              758,
              1265,
              991,
              1128,
              1075,
              1271,
              842,
              1408
            ],
            "text": "user"
          }
        ]
      },
      {
        "boundingBox": [
          2020,
          1274,
          2132,
          1284,
          2128,
          1340,
          2014,
          1332
        ],
        "text": "CK",
        "words": [
          {
            "boundingBox": [
              2014,
              1283,
              2114,
              1281,
              2102,
              1342,
              2002,
              1343
            ],
            "text": "CK"
          }
        ]
      },
      {
        "boundingBox": [
          296,
          1736,
          602,
          1586,
          644,
          1676,
          338,
          1826
        ],
        "text": "Speaker",
        "words": [
          {
            "boundingBox": [
              231,
              1774,
              572,
              1593,
              688,
              1649,
              347,
              1831
            ],
            "text": "Speaker"
          }
        ]
      },
      {
        "boundingBox": [
          1656,
          1634,
          1834,
          1606,
          1846,
          1688,
          1668,
          1716
        ],
        "text": "Verify",
        "words": [
          {
            "boundingBox": [
              1622,
              1622,
              1873,
              1612,
              1859,
              1694,
              1608,
              1705
            ],
            "text": "Verify"
          }
        ]
      },
      {
        "boundingBox": [
          2226,
          1704,
          2414,
          1634,
          2436,
          1694,
          2248,
          1764
        ],
        "text": "8 - 2 - 1",
        "words": [
          {
            "boundingBox": [
              2227,
              1684,
              2274,
              1675,
              2262,
              1752,
              2215,
              1762
            ],
            "text": "8"
          },
          {
            "boundingBox": [
              2288,
              1672,
              2335,
              1662,
              2323,
              1740,
              2276,
              1750
            ],
            "text": "-"
          },
          {
            "boundingBox": [
              2330,
              1663,
              2378,
              1654,
              2366,
              1732,
              2318,
              1741
            ],
            "text": "2"
          },
          {
            "boundingBox": [
              2373,
              1655,
              2420,
              1646,
              2408,
              1723,
              2361,
              1733
            ],
            "text": "-"
          },
          {
            "boundingBox": [
              2392,
              1651,
              2439,
              1642,
              2427,
              1720,
              2380,
              1729
            ],
            "text": "1"
          }
        ]
      },
      {
        "boundingBox": [
          284,
          1946,
          892,
          1538,
          960,
          1640,
          352,
          2048
        ],
        "text": "Please Work 4 M",
        "words": [
          {
            "boundingBox": [
              263,
              1958,
              532,
              1786,
              593,
              1889,
              324,
              2061
            ],
            "text": "Please"
          },
          {
            "boundingBox": [
              519,
              1794,
              755,
              1644,
              816,
              1747,
              580,
              1898
            ],
            "text": "Work"
          },
          {
            "boundingBox": [
              762,
              1639,
              829,
              1596,
              890,
              1700,
              823,
              1743
            ],
            "text": "4"
          },
          {
            "boundingBox": [
              816,
              1605,
              883,
              1562,
              944,
              1665,
              877,
              1708
            ],
            "text": "M"
          }
        ]
      },
      {
        "boundingBox": [
          2920,
          1920,
          3088,
          1916,
          3090,
          2014,
          2922,
          2018
        ],
        "text": "AAA",
        "words": [
          {
            "boundingBox": [
              2884,
              1922,
              3119,
              1922,
              3116,
              2016,
              2881,
              2016
            ],
            "text": "AAA"
          }
        ]
      },
      {
        "boundingBox": [
          1678,
          2098,
          1796,
          2108,
          1792,
          2152,
          1674,
          2142
        ],
        "text": "Name",
        "words": [
          {
            "boundingBox": [
              1654,
              2097,
              1810,
              2110,
              1798,
              2155,
              1642,
              2142
            ],
            "text": "Name"
          }
        ]
      },
      {
        "boundingBox": [
          2720,
          2074,
          3090,
          2022,
          3104,
          2114,
          2734,
          2166
        ],
        "text": "270 AAB",
        "words": [
          {
            "boundingBox": [
              2690,
              2082,
              2875,
              2056,
              2869,
              2150,
              2683,
              2176
            ],
            "text": "270"
          },
          {
            "boundingBox": [
              2875,
              2056,
              3112,
              2022,
              3106,
              2116,
              2869,
              2150
            ],
            "text": "AAB"
          }
        ]
      },
      {
        "boundingBox": [
          1048,
          2234,
          1152,
          2236,
          1150,
          2272,
          1046,
          2270
        ],
        "text": "edge",
        "words": [
          {
            "boundingBox": [
              1034,
              2239,
              1153,
              2232,
              1157,
              2274,
              1038,
              2281
            ],
            "text": "edge"
          }
        ]
      }
    ]
  }
}
```

---

## Content Moderation

#### Image
- **Evaluate** - Returns probabilities of the image containing racy or adult content.
- **Find Faces**
- **Match** - Fuzzily match an image against a custom list
- **OCR** - Returns any text found in the image for the language specified.

```bash
$ curl -v -X POST "https://westus.api.cognitive.microsoft.com/contentmoderator/moderate/v1.0/ProcessImage/FindFaces?CacheImage=false" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" -H "Content-Type:image/jpeg" --data-binary @swimsuit.jpg >swimsuit-face.json
```

#### Text
- **Detect Language** - Returns the ISO 639-3 code for the predominant language comprising the submitted text
- **Screen** - Detects profanity in more than 100 languages and match against custom and shared blacklists

#### Screen

- Autocorrects
- Detects PII (email, phone, SSN)
- Classification Score
  - Sexually Explicit
  - Sexually Suggestive
  - Offensive

```bash
$ curl -v -X POST "https://westus.api.cognitive.microsoft.com/contentmoderator/moderate/v1.0/ProcessText/Screen?autocorrect=true&PII=true&classify=true" -H "Content-Type: text/plain" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" --data-ascii "text to be screened here with PII like an email@address.net" >text-screen.json
```

#### Video

Integrated with Azure Media Services

#### List Management
- Image
- Image Lists
- Term
- Term Lists

#### Job

- Create
- Get

#### Review

- Add video frames
- Add video transcript
- Add video transcript moderation result
- Create
- Get
- Get video frames
- Publish video review

#### Workflow

- Create
- Update
- Get
- Get All

---

## Custom Vision Service

#### Face API

- CreateImagesFrom(Data|Files|Predictions|Urls)
- CreateProject
- CreateTag
- DeleteImages
- DeleteImageTags
- DeleteIteration
- DeletePrediction
- DeleteProject
- DeleteTag
- ExportIteration
- GetAccountInfo
- GetDomain
- GetDomains
- GetExports
- GetIteration
- GetIterationPerformance
- GetIterations
- GetProject
- GetProjects
- GetTag
- GetTaggedImages
- GetTags
- GetUntaggedImages
- PostImageTags
- QueryPredictionResults
- QuickTestImage
- QuickTestImageUrl
- TrainProject
- UpdateIteration
- UpdateProject
- UpdateTag


https://customvision.ai/projects/70538e10-bcdb-4df7-93f0-a586688016c3#/manage


```bash
$ curl -v -X GET "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.2/Training/domains" -H "Training-key: your_key_goes_here" > domains.json
```

```json
[
  {
    "Id": "ee85a74c-405e-4adc-bb47-ffa8ca0c9f31",
    "Name": "General",
    "Exportable": false
  },
  {
    "Id": "c151d5b5-dd07-472a-acc8-15d29dea8518",
    "Name": "Food",
    "Exportable": false
  },
  {
    "Id": "ca455789-012d-4b50-9fec-5bb63841c793",
    "Name": "Landmarks",
    "Exportable": false
  },
  {
    "Id": "b30a91ae-e3c1-4f73-a81e-c270bff27c39",
    "Name": "Retail",
    "Exportable": false
  },
  {
    "Id": "45badf75-3591-4f26-a705-45678d3e9f5f",
    "Name": "Adult",
    "Exportable": false
  },
  {
    "Id": "0732100f-1a38-4e49-a514-c9b44c697ab5",
    "Name": "General (compact)",
    "Exportable": true
  },
  {
    "Id": "b5cfd229-2ac7-4b2b-8d0a-2b0661344894",
    "Name": "Landmarks (compact)",
    "Exportable": true
  },
  {
    "Id": "6b4faeda-8396-481b-9f8b-177b9fa3097f",
    "Name": "Retail (compact)",
    "Exportable": true
  }
]
```

```bash
$ curl -v -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.2/Training/projects?name=SeanK" -H "Training-key: your_key_goes_here" --data-ascii "" > createProject.json
```

```json
{
  "Id": "70538e10-bcdb-4df7-93f0-a586688016c3",
  "Name": "SeanK",
  "Description": "",
  "Settings": {
    "DomainId": "ee85a74c-405e-4adc-bb47-ffa8ca0c9f31",
    "UseNegativeSet": true,
    "ClassificationType": "MultiLabel",
    "DetectionParameters": null
  },
  "CurrentIterationId": "e45f40a6-6bc6-4e2e-b3e2-40552288a730",
  "Created": "2018-05-04T19:23:20.4666667",
  "LastModified": "2018-05-04T19:23:20.4666667",
  "ThumbnailUri": null
}
```

```bash
$ curl -v -X DELETE "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.2/Training/projects/70538e10-bcdb-4df7-93f0-a586688016c3" -H "Training-key: your_key_goes_here"
```

```bash
$ curl -v -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.2/Training/projects/70538e10-bcdb-4df7-93f0-a586688016c3/tags?name=Winners" -H "Training-key: your_key_goes_here" --data-ascii "" > createTag.json
```

```json
{
  "Id": "c996a51f-a70d-4e10-b803-13397876b40b",
  "Name": "Winners",
  "Description": null,
  "ImageCount": 0
}
```

```bash
$ curl -v -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.2/Training/projects/70538e10-bcdb-4df7-93f0-a586688016c3/images?tagIds=c996a51f-a70d-4e10-b803-13397876b40b" -H "Training-key: your_key_goes_here" -F "image01=@image-01.jpg" -F "image02=@image-02.jpg" -F "image03=@image-03.jpg" -F "image04=@image-04.jpg" -F "image05=@image-05.jpg" -F "image06=@image-06.jpg" -F "image07=@image-07.jpg" -F "image08=@image-08.jpg" -F "image09=@image-09.jpg" -F "image10=@image-10.jpg" -F "image11=@image-11.jpg" -F "image12=@image-12.jpg" -F "image13=@image-13.jpg" -F "image14=@image-14.jpg" -F "image15=@image-15.jpg" -F "image16=@image-16.jpg" -F "image17=@image-17.jpg" -F "image18=@image-18.jpg" -F "image19=@image-19.jpg" >train.json

$ curl -v -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.2/Training/projects/70538e10-bcdb-4df7-93f0-a586688016c3/train" -H "Training-key: your_key_goes_here" --data-ascii "" > train.json
```

```json
{
    "Id":"80df8cf1-eb80-418a-bead-17b04bc168de",
    "Name":"Iteration 2",
    "IsDefault":false,
    "Status":"Training",
    "Created":"2018-05-04T19:38:56.6166667",
    "LastModified":"2018-05-04T20:28:33.0191429",
    "ProjectId":"70538e10-bcdb-4df7-93f0-a586688016c3",
    "Exportable":false,
    "DomainId":null
}
```

```bash
$ curl -v -X GET "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.2/Training/projects/70538e10-bcdb-4df7-93f0-a586688016c3/iterations/80df8cf1-eb80-418a-bead-17b04bc168de" -H "Training-key: your_key_goes_here" >trainingStatus.json
```

```json
{
    "Id":"80df8cf1-eb80-418a-bead-17b04bc168de",
    "Name":"Iteration 2",
    "IsDefault":false,
    "Status":"Completed",
    "Created":"2018-05-04T19:38:56.6166667",
    "LastModified":"2018-05-04T20:28:50.3050096",
    "TrainedAt":"2018-05-04T20:28:50.3050096",
    "ProjectId":"70538e10-bcdb-4df7-93f0-a586688016c3",
    "Exportable":false,
    "DomainId":"ee85a74c-405e-4adc-bb47-ffa8ca0c9f31"
}
```

```bash
curl -v -X POST "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.2/Training/projects/70538e10-bcdb-4df7-93f0-a586688016c3/quicktest/image?iterationId=80df8cf1-eb80-418a-bead-17b04bc168de" -H "Training-key: your_key_goes_here" -F "image=@test-1.jpg" >test1.json
```

```json
{
    "Id":"961acc84-f209-4862-bd43-a7b574666a52",
    "Project":"70538e10-bcdb-4df7-93f0-a586688016c3",
    "Iteration":"80df8cf1-eb80-418a-bead-17b04bc168de",
    "Created":"2018-05-04T20:30:05.6189491Z",
    "Predictions":[
        {
            "TagId":"c996a51f-a70d-4e10-b803-13397876b40b",
            "Tag":"Winners",
            "Probability":0.217956811
        },
        {
            "TagId":"7eab77d4-38db-44b0-82e4-45b59568418c",
            "Tag":"Losers",
            "Probability":0.05441097
        }
    ]
}
```

---

## Face

#### Face API

- Face
- FaceList
- LargeFaceList
- LargePersonGroup
- LargePersonGroup Person
- PersonGroup
- PresonGroup Person

#### Face

- Detect
- Find Similar
- Group
- Identify
- Verify

#### Detect

- FaceId
- Landmarks
- Attributes
  - age, gender, headPose, smile, facialHair, glasses,
  - emotion, hair, makeup, occlusion, accessories, blur,
  - exposure, noise

```bash
$ curl -v -X POST "https://westus.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=true&returnFaceLandmarks=true&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" -H "Content-Type:application/octet-stream" --data-binary @image.jpg >image-detect.json
```

```json
[
  {
    "faceId": "fb9fd113-5e44-4568-915f-a065c747315d",
    "faceRectangle": { "top": 440, "left": 360, "width": 552, "height": 552 },
    "faceLandmarks": {
      "pupilLeft": { "x": 497.6, "y": 593.9 },
      "pupilRight": { "x": 762.3, "y": 587.9 },
      "noseTip": { "x": 636.1, "y": 757.9 },
      "mouthLeft": { "x": 526.2, "y": 837.8 },
      "mouthRight": { "x": 757.7, "y": 829.7 },
      "eyebrowLeftOuter": { "x": 397.8, "y": 556.3 },
      "eyebrowLeftInner": { "x": 571.9, "y": 563.3 },
      "eyeLeftOuter": { "x": 452.7, "y": 599.9 },
      "eyeLeftTop": { "x": 494.4, "y": 580.5 },
      "eyeLeftBottom": { "x": 494.2, "y": 615.5 },
      "eyeLeftInner": { "x": 540.6, "y": 606.1 },
      "eyebrowRightInner": { "x": 676.2, "y": 564.2 },
      "eyebrowRightOuter": { "x": 856.7, "y": 540.6 },
      "eyeRightInner": { "x": 717.8, "y": 604.5 },
      "eyeRightTop": { "x": 757.9, "y": 575.3 },
      "eyeRightBottom": { "x": 765.2, "y": 608.1 },
      "eyeRightOuter": { "x": 810.1, "y": 590.8 },
      "noseRootLeft": { "x": 594.5, "y": 612.3 },
      "noseRootRight": { "x": 669, "y": 610.2 },
      "noseLeftAlarTop": { "x": 577.4, "y": 713.2 },
      "noseRightAlarTop": { "x": 689, "y": 709.9 },
      "noseLeftAlarOutTip": { "x": 560, "y": 760.7 },
      "noseRightAlarOutTip": { "x": 715.8, "y": 750.9 },
      "upperLipTop": { "x": 637.1, "y": 832.7 },
      "upperLipBottom": { "x": 639.8, "y": 859.4 },
      "underLipTop": { "x": 640.4, "y": 864.3 },
      "underLipBottom": { "x": 644.8, "y": 900.5 }
    },
    "faceAttributes": {
      "smile": 1,
      "headPose": { "pitch": 0, "roll": -2.2, "yaw": -0.3 },
      "gender": "female",
      "age": 37.7,
      "facialHair": { "moustache": 0, "beard": 0, "sideburns": 0 },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 1,
        "neutral": 0,
        "sadness": 0,
        "surprise": 0
      },
      "blur": { "blurLevel": "medium", "value": 0.46 },
      "exposure": { "exposureLevel": "goodExposure", "value": 0.62 },
      "noise": { "noiseLevel": "low", "value": 0.06 },
      "makeup": { "eyeMakeup": true, "lipMakeup": true },
      "accessories": [],
      "occlusion": { "foreheadOccluded": false, "eyeOccluded": false, "mouthOccluded": false },
      "hair": {
        "bald": 0.02,
        "invisible": false,
        "hairColor": [
          { "color": "black", "confidence": 0.99 },
          { "color": "brown", "confidence": 0.96 },
          { "color": "other", "confidence": 0.79 },
          { "color": "red",   "confidence": 0.07 },
          { "color": "gray",  "confidence": 0.04 },
          { "color": "blond", "confidence": 0.02 }
        ]
      }
    }
  }
]
```


#### Find Similar

- Find similar has two working modes, "matchPerson" and "matchFace". "matchPerson" is the default mode that it tries to find faces of the same person as possible by using internal same-person thresholds. "matchFace" mode ignores same-person thresholds and returns ranked similar faces anyway, even the similarity is low.

```bash
$ curl -v -X POST "https://westus.api.cognitive.microsoft.com/face/v1.0/findsimilars" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" -H "Content-Type: application/json" --data-ascii "{\"faceId\":\"fb9fd113-5e44-4568-915f-a065c747315d\",\"faceIds\":[\"fb9fd113-5e44-4568-915f-a065c747315d\",\"e1f5c068-e41b-4c06-a854-49681ee8f9d1\",\"197aff3c-5035-483e-935b-abbb00cde462\",\"3a328139-dc3f-4fc6-b1d8-9507cf93276f\",\"0c9b0578-78a7-4589-a6c7-c39191a521d7\",\"0da422cc-535f-42ab-ab05-d6a96a55473b\",\"c904722b-2672-4eaf-9c5d-a59b3d5a78e4\",\"1530d094-597f-4ea1-a4ec-16b601802e19\",\"3583a60f-8240-44ca-b1d8-abb01b81a647\",\"125d0537-fadd-48dc-a0a1-64aa0218fc0a\",\"8071d1c7-6ba7-4877-a161-95a79a80020d\",\"d0ffc832-da05-46d1-9aca-e719614beb1d\",\"c8f8ac66-ce14-48a1-85ce-369e71d6438a\",\"c318971e-a736-4991-be32-09f70c1e8056\",\"12dd9646-838e-426b-adc0-0fa3d0bff416\"],\"maxNumOfCandidatesReturned\":20,\"mode\":\"matchPerson\"}" >similar.json
```

```json
[
  {
    "faceId": "fb9fd113-5e44-4568-915f-a065c747315d",
    "confidence": 1
  },
  {
    "faceId": "197aff3c-5035-483e-935b-abbb00cde462",
    "confidence": 0.7494797
  },
  {
    "faceId": "e1f5c068-e41b-4c06-a854-49681ee8f9d1",
    "confidence": 0.7215838
  }
]
```

#### Groups

Divide candidate faces into groups based on face similarity.

```bash
$ curl -v -X POST "https://westus.api.cognitive.microsoft.com/face/v1.0/group" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" -H "Content-Type: application/json" --data-ascii "{\"faceIds\":[\"fb9fd113-5e44-4568-915f-a065c747315d\",\"e1f5c068-e41b-4c06-a854-49681ee8f9d1\",\"197aff3c-5035-483e-935b-abbb00cde462\",\"3a328139-dc3f-4fc6-b1d8-9507cf93276f\",\"0c9b0578-78a7-4589-a6c7-c39191a521d7\",\"0da422cc-535f-42ab-ab05-d6a96a55473b\",\"c904722b-2672-4eaf-9c5d-a59b3d5a78e4\",\"1530d094-597f-4ea1-a4ec-16b601802e19\",\"3583a60f-8240-44ca-b1d8-abb01b81a647\",\"125d0537-fadd-48dc-a0a1-64aa0218fc0a\",\"8071d1c7-6ba7-4877-a161-95a79a80020d\",\"d0ffc832-da05-46d1-9aca-e719614beb1d\",\"c8f8ac66-ce14-48a1-85ce-369e71d6438a\",\"c318971e-a736-4991-be32-09f70c1e8056\",\"12dd9646-838e-426b-adc0-0fa3d0bff416\"]}" >groups.json
```

```json
{
  "groups": [
    [
      "125d0537-fadd-48dc-a0a1-64aa0218fc0a",
      "8071d1c7-6ba7-4877-a161-95a79a80020d",
      "d0ffc832-da05-46d1-9aca-e719614beb1d",
      "c8f8ac66-ce14-48a1-85ce-369e71d6438a"
    ],
    [
      "1530d094-597f-4ea1-a4ec-16b601802e19",
      "0da422cc-535f-42ab-ab05-d6a96a55473b",
      "3a328139-dc3f-4fc6-b1d8-9507cf93276f"
    ],
    [
      "197aff3c-5035-483e-935b-abbb00cde462",
      "fb9fd113-5e44-4568-915f-a065c747315d",
      "e1f5c068-e41b-4c06-a854-49681ee8f9d1"
    ]
  ],
  "messyGroup": [
    "0c9b0578-78a7-4589-a6c7-c39191a521d7",
    "c904722b-2672-4eaf-9c5d-a59b3d5a78e4",
    "3583a60f-8240-44ca-b1d8-abb01b81a647",
    "c318971e-a736-4991-be32-09f70c1e8056",
    "12dd9646-838e-426b-adc0-0fa3d0bff416"
  ]
}
```

#### Identify

1-to-many identification to find the closest matches of the specific query person face from a person group or large person group.


#### Verify

Verify whether two faces belong to a same person or whether one face belongs to a person.

```bash
curl -v -X POST "https://westus.api.cognitive.microsoft.com/face/v1.0/verify" -H "Ocp-Apim-Subscription-Key: your_key_goes_here" -H "Content-Type: application/json" --data-ascii "{\"faceId1\": \"3583a60f-8240-44ca-b1d8-abb01b81a647\",\"faceId2\": \"3a328139-dc3f-4fc6-b1d8-9507cf93276f\"}" >verify.json
```

```json
{
  "isIdentical": false,
  "confidence": 0.40676
}
```

#### FaceList and LargeFaceList

- Facelist supports up to 1,000 faces, LargeFaceList suports up to 1,000,000 faces
- 1 Photo per Face
- Used with Find Similar
- Both FaceList and LargeFaceList can: Add Face, Create, Delete, Delete Face, Get, List, Update
- LargeFaceList can also: Get Face, Get Training Status, List Face, Train, Update Face

#### PersonGroup and LargePersonGroup

- PersonGroup supports up to 10,000 persons per group, LargePersonGroup supports up to 1,000,000 persons per group
- Up to 248 Photos per Person
- Used with Identify and Verify
- Both PersonGroup and LargePersonGroup can: Create Group, Delete Group, Get Group Info, Get Training Status, List Groups, Train Group, Update Group,

---

## VideoIndexer

- Add linguistic training data
- Create Brand
- Create linguistic model
- Create linguistic training data group
- Delete Brand
- Delete Breakdown
- Delete Linguistic Model
- Delete linguistic training data
- Delete linguistic training data group
- Get Access Token
- Get Accounts
- Get Brand
- Get Brands
- Get Breakdown
- Get Insights Widget Url
- Get Insights Widget Url By External Id
- Get linguistic model
- Get linguistic training data
- Get linguistic training data group
- Get Player Widget Url
- Get Processing State
- Get Vtt Url
- Re-Index Breakdown
- Re-Index Breakdown By External Id
- Search
- Update Bing brands activation state
- Update Brand
- Update Face Name
- Update linguistic training data
- Update linguistic training data group
- Update Transcript
- Upload
