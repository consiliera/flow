###
These files are made available to you on an as-is and restricted basis, and may only be redistributed or sold to any third party as expressly indicated in the Terms of Use for Seed Vault.
###
Seed Vault Code (c) Botanic Technologies, Inc. Used under license.
###

# Brief write up on flow:

## Flow:
- is a representation of conversation between bot & user
- is represented in the form of dialogs
- each dialog is represented by a node
- will have collection of nodes
- will have a purpose, probably an intent to perform
- will look like a graph

## Attributes of a flow :

- ###  id:
      an unique identifier, usually UUID, automatically generated by the author tool

- ###  name:
      friendly name which signifies the purpose, given by author

- ###  nodes:
      an array of individual node(s)

## Node:
- each node represents a dialog either from bot or user
- will have a purpose, either to show to user or take input from user or do some processing
- each node may have one or more paths leading to other nodes

### Attributes of a node :

- ###  id:
			an unique identifier(with in the flow) to identify a node, automatically generated by author tool

- ###  name:
			friendly name which signifies the purpose of this node, given by author

- ###  type: 
			purpose of this node (eg., root, message, choice, question, card, process and end), given by author

- ###  subtype:
			will have more information regarding the type of the node, given by author

- ###  msg:
			a message which need to be shown to user ( or an array of messages from which randomly a message need to be picked), given by author

- ###  fieldId:
      an entity, the value for which user has to provide, which later will be used while performing the intent, given by author

- ###  formId:
      an identifier which represents a form, on which logically related fields will serve a purpose, given by author

- ###  Connections:
			possible paths that lead to other nodes after executing current node, given by author

## Connection:
- a single path that lead to another node in the flow after executing current node, given by author

### Attributes of a Connection :
      
- ###  default:
        which indicates where to move next, given by author

- ###  If:
        which indicates where to move next, provided a condition is satisfied, given by author

- ###  name:
        human readable or understandable name given for the connection

- ###  isButton:
        which states whether this connection need to be shown to user in the form of a button, so that user can click it to proceed further in that path

### Attributes of a If :

- ###  pre:
        which indicates that there is a pre-condition that need to be evaluated first, if this condition is successful only then the "if" condition will be evaluated, given by author

- ###  op:
        an operator (for eg., logical or something completely new which represents an operation), given by author
        
- ###  value:
        a value(s) to which comparison need to happen, given by author
        
- ###  context:
        which signifies about what context this check need to be done, given by author. for eg., "sentimentAnalysis" is used to check the sentiment analysis value and take decision.
        
- ###  then:
        which indicates where to move next on satisfying the condition, given by author

### Attributes of a Pre :

- ###  varname:
        which signifies the variable name ie., a field id, given by author

- ###  op:
        an operator (for eg., logical or something completely new which represents an operation), given by author
        
- ###  value:
        a value(s) to which comparison need to happen, given by author

### eg., of *default* in json:

    {
      "default": "ffb8"
    }

### eg., of *if* in json:

    {
      "context": "",
      "op": "eq",
      "value": [
        "green"
      ],
      "then": "8193"
    }

    {
      "context": "sentimentAnalysis",
      "op": "eq",
      "value": [
        "-4"
      ],
      "then": "7ebe"
    }

### eg., of *if* with *pre* in json:

    {
      "context": "userInput",
      "op": "pattern",
      "value": [
        "*"
      ],
      "pre": {
        "varName": "bookingId",
        "op": "eq",
        "value": "null"
      },
      "then": "e41c"
    }

### eg., of *connection* in json:

    {
      "if": {
        "context": "userInput",
        "op": "pattern",
        "value": [
          "*"
        ],
        "pre": {
          "varName": "bookingId",
          "op": "eq",
          "value": "null"
        },
        "then": "e41c"
      }
    }

## Types of nodes:

- ###  root:
			purpose is to say from which node execution need to be started, automatically generated by author tool when a flow is created

- ###  message:
      purpose is to show user a message

- ###  choice:
			purpose is to ask user a question with possible choices to choose from

- ###  card:
			purpose is to show user some content, probably text or media (image, audio or video)

- ###  process:
			purpose is to do some processing in the background, for eg., lets assume the purpose of a flow is to ask few questions to user, take answers and send its findings via an email. The email sending sub system needs receipents, subject, body information to form an email and send. So the flow will have questions to gather information along with a process node at the end which will have all the required information for sending an email properly.

- ###  flowId:
      purpose is to inform that its time to execute another flow      

- ###  end:
			purpose is to say end of flow, but this node itself doesn't exist in the flow, hence will not have any attributes

### eg., of node - *root* in json:

    {
      "id": "root-node",
      "name": "flow71",
      "type": "root",
      "connections": [
        {
          "default": "304e"
        }
      ]
    }

### eg., of node - *message* in json:

    {
      "id": "dd9b",
      "type": "message",
      "subType": "text",
      "name": "",
      "msg": [
        "do you want to see a video, form or buttons?"
      ],
      "connections": [
        {
          "if": {
            "context": "",
            "op": "eq",
            "value": [
              "button"
            ],
            "then": "0e1f"
          }
        },
        {
          "if": {
            "context": "",
            "op": "eq",
            "value": [
              "form"
            ],
            "then": "d060"
          }
        },
        {
          "if": {
            "context": "",
            "op": "eq",
            "value": [
              "video"
            ],
            "then": "7392"
          }
        },
        {
          "default": "end"
        }
      ]
    }

### eg., of node - *choice* in json:

    {
      "id": "304e",
      "type": "choice",
      "subType": "text",
      "name": "",
      "msg": [
        "Please choose one"
      ],
      "data": [
        "black",
        "green",
        "white"
      ],
      "connections": [
        {
          "if": {
            "context": "",
            "op": "eq",
            "value": [
              "green"
            ],
            "then": "8193"
          }
        },
        {
          "if": {
            "context": "",
            "op": "eq",
            "value": [
              "white"
            ],
            "then": "011e"
          }
        },
        {
          "if": {
            "context": "",
            "op": "eq",
            "value": [
              "black"
            ],
            "then": "287e"
          }
        },
        {
          "if": {
            "context": "",
            "op": "eq",
            "value": [
              "no"
            ],
            "then": "599e"
          }
        },
        {
          "default": "end"
        }
      ]
    }

### eg., of node - *card* in json:

    {
      "id": "7392",
      "type": "card",
      "subType": "video",
      "name": "",
      "msg": [
        "here is the video"
      ],
      "info": {
        "type": "video",
        "title": "video title",
        "subtitle": "video subtitle",
        "info": {
          "image_uri": "https://example.com/cdkydjg.jpg",
          "media_uri": "http://example.com/demos/sample-videos/small.mp4",
          "profile": "hd",
          "aspect": "4:3",
          "autostart": true,
          "autoloop": true,
          "mute": false,
          "controls": true,
          "alt_text": ""
        }
      },
      "connections": [
        {
          "default": "end"
        }
      ]
    }

### eg., of node - *process* in json:

    {
      "id": "e342",
      "type": "process",
      "subType": "sendEmail",
      "name": "",
      "msg": [
        "Thanks!"
      ],
      "info": {
        "recipient": "kiran@example.io,johnm@example.io,nicolas@example.io",
        "subject": "email subject",
        "body": "This is the form:",
        "formId": "form1"
      },
      "connections": [
        {
          "default": "end"
        }
      ]
    }

### eg., of node - *flowId* in json:

    {
      "id": "0e1f",
      "type": "flowId",
      "name": "",
      "msg": [],
      "flowId": 71,
      "connections": [
        {
          "default": "end"
        }
      ]
    }

### eg., of *flow* in json:

    {
      "id": 69,
      "name": "flow69",
      "nodes": [
        {
          "id": "root-node",
          "name": "flow69",
          "type": "root",
          "connections": [
            {
              "default": "dd9b"
            }
          ]
        },
        {
          "type": "message",
          "subType": "text",
          "name": "",
          "msg": [
            "do you want to see a video, form or buttons?"
          ],
          "id": "dd9b",
          "connections": [
            {
              "if": {
                "context": "",
                "op": "eq",
                "value": [
                  "button"
                ],
                "then": "0e1f"
              }
            },
            {
              "if": {
                "context": "",
                "op": "eq",
                "value": [
                  "form"
                ],
                "then": "d060"
              }
            },
            {
              "if": {
                "context": "",
                "op": "eq",
                "value": [
                  "video"
                ],
                "then": "7392"
              }
            },
            {
              "default": "end"
            }
          ]
        },
        {
          "type": "card",
          "subType": "video",
          "name": "",
          "msg": [
            "here is the video"
          ],
          "info": {
            "type": "video",
            "title": "video title",
            "subtitle": "video subtitle",
            "info": {
              "image_uri": "https://example.com/cdkydjg.jpg",
              "media_uri": "http://example.com/demos/sample-videos/small.mp4",
              "profile": "hd",
              "aspect": "4:3",
              "autostart": true,
              "autoloop": true,
              "mute": false,
              "controls": true,
              "alt_text": ""
            }
          },
          "id": "7392",
          "connections": [
            {
              "default": "end"
            }
          ]
        },
        {
          "type": "flowId",
          "name": "",
          "msg": [],
          "flowId": 70,
          "id": "d060",
          "connections": [
            {
              "default": "end"
            }
          ]
        },
        {
          "type": "flowId",
          "name": "",
          "msg": [],
          "flowId": 71,
          "id": "0e1f",
          "connections": [
            {
              "default": "end"
            }
          ]
        }
      ]
    }
