Override Instructions
---------------------
### Template JSON
Given the JSON in `base-iag-template.json`

```javascript
{
  "api_definition": {
    "name": "Test",
    "auth": {
      "auth_header_name": "authorization"
    },
    "definition": {
      "location": "header",
      "key": ""
    },
    "proxy": {
      "target_url": "http://httpbin.org/"
    },
    "version_data": {
      "use_extended_paths": true,
      "not_versioned": true,
      "versions": {
        "Default": {
          "expires": "",
          "name": "Default",
          "paths": {
            "ignored": [],
            "white_list": [],
            "black_list": []
          },
          "extended_paths": {
            "ignored": [
              {
                "path": "/test-path/",
                "method_actions": {
                  "GET": {
                    "action": "no_action",
                    "code": 200,
                    "data": "",
                    "headers": {}
                  }
                }
              },
              {
                "path": "/test-path/reply",
                "method_actions": {
                  "GET": {
                    "action": "reply",
                    "code": 200,
                    "data": "{\"foo\":\"bar\"}",
                    "headers": {
                      "x-test": "test"
                    }
                  }
                }
              }
            ],
            "white_list": [],
            "black_list": []
          },
          "use_extended_paths": true
        }
      }
    },
    "use_oauth2": false,
    "oauth_meta": {
      "auth_login_redirect": "",
      "allowed_access_types": [],
      "allowed_authorize_types": [
        "token"
      ]
    },
    "notifications": {
      "shared_secret": "",
      "oauth_on_keychange_url": ""
    },
    "enable_ip_whitelisting": true,
    "allowed_ips": [
      "127.0.0.1"
    ],
    "use_keyless": false,
    "enable_signature_checking": false,
    "use_basic_auth": false,
    "active": true,
    "enable_batch_request_support": true
  },
  "hook_references": [
    {
      "event_name": "QuotaExceeded",
      "hook": {
        "api_model": {},
        "id": "54be6c0beba6db07a6000002",
        "org_id": "54b53d3aeba6db5c35000002",
        "name": "Test Post",
        "method": "POST",
        "target_path": "http://httpbin.org/post",
        "template_path": "",
        "header_map": {
          "x-tyk-test": "123456"
        },
        "event_timeout": 0
      },
      "event_timeout": 60
    }
  ]
}
```
### Override JSON Sample
This is a sample of Override JSON in `base-override-iag-template.json`. The root of Override JSON is a JSONArray. Each element in the array is a JSONObject with fields `path`, `type` and `value`. `path` and `value` are compulsory fields for all Override Instructions. `type` is compulsory for instructions to add an element into JSONArray and to add a field in a JSONObject (modifying the JSONObject directly), while for other kinds of instructions `type` is an optonal field. The `path` field is actually a JSONPath.  Refer <a href="https://github.com/json-path/JsonPath" target="_blank">JSONPath</a> to know more about JSONPath.

```javascript
{
  "overrideInstructions": [
    {
      "path" : "$.api_definition.version_data.versions.Default.paths.black_list",
      "type" : "list.string",
      "value" : "value1"
    },
    {
      "path" : "$.api_definition.version_data.versions.Default.paths.black_list",
      "type" : "list.string",
      "value" : "value2"
    },
    {
      "path" : "$.hook_references",
      "type" : "list",
      "value" : "{\"No\":\"17\",\"Name\":\"Andrew\"}"
    },
    {
      "path" : "$.api_definition.version_data.versions.Default.paths.black_list",
      "type" : "list",
      "value" : "{\"No\":\"17\",\"Name\":\"Andrew\"}"
    },
    {
      "path" : "$.api_definition.version_data.versions.Default.paths.white_list",
      "type" : "list",
      "value" : "{\"No\":\"17\",\"Name\":\"Andrew\"}"
    },
    {
      "path" : "$.hook_references[0].hook.api_model",
      "type" : "object",
      "value" : "{\"Name\":\"Model\"}"
    }]
}
```

### Type of Instructions
All the following operations are applicable for nested JSONObject and nested JSONArray. These oprations are used to Override the JSON in the file `base-iag-template.json`

1.Updating the value of existing field whose value is a string <br/>
2.Adding an element into JSONArray <br/>
3.Updating a particular element in JSONArray <br/>
4.Adding new field to a JSONObject <br/>

An Example to update each field is shown in the tables below along with appropriate explanation about the instruction. In the following table. `path` represents path field in each object in `overrideInstructions` JSONArray. Similarly `value` represents value field. In the table, `value` column is for illustration purpose. Any such valid value is allowed. More details are provided in the `Explanation of the instruction` column. `type` represents type field. In some instructions, `type` is not necessary. For those instructions `NA` (Not applicable) is given in `type` column of the table. For such instructions `type` field can be avoided in JSONObject. **The values for the fields `path` and `type` should be enclosed in doube quotes ("") as shown in the example OverrideInstructions JSON above.**  

### 1. Updating the value of existing field

| path| Explanation of the instruction| value | type |  
| :---| :-----------------------------|:-----:|:----:|
|$.api_definition.name|The value is a string <br/> It is name of the API | "TestAPI" |`NA` |
|$.api_definition.auth.auth_header_name| The value is a string. <br/> Auth header name | "authentication" | `NA` |
|$.api_definition.definition.location| The value is a string. | "location header" | `NA` |
|$.api_definition.definition.key| The value is a string. | "key1" | `NA` |
|$.api_definition.proxy.target_url| The value is a string.<br/> It is valid URL| "http://google.com/" | `NA` |
|$.api_definition.version_data.versions.Default.expires| The value is a string. | "yes" | `NA` |
|$.api_definition.version_data.versions.Default.name| The value is a string. | "DefaultName" | `NA` |
|$.api_definition.oauth_meta.auth_login_redirect| The value is a string. | "abc123" | `NA` |
|$.api_definition.notifications.shared_secret|The value is a string. | "abc123" | `NA` |
|$.api_definition.notifications.oauth_on_keychange_url|The value is a string. | "abc123" | `NA` |
|$.api_definition.version_data.versions.Default.extended_paths.ignored[0].path| "/test-path-workflow/" | `NA` |

### 2. Adding an element into JSONArray

The overrideInstructions to insert into a JSONArray containing JSONObjects, have the `type` as `"list"`. Refer the example TYK JSON given below to know about the general structure of such JSONObjects. In these JSONObjects, The Inner JSONs can have custom structure with more fields as required. In the instructions in table, the value is a stringified JSONObject. Follow the Java syntax for strings. Take care of Escape Sequence such as \\", \\n and \\\\ <br/> **While trying the example value provided here, do not copy paste. Type the value in the `value` field without line feeds**

| path| Explanation of the instruction| value | type |  
| :----| :-----------------------------|:-----|:----:|
|$.api_definition.version_data.versions.Default.paths.ignored|The value is s string. <br/> It is an IP address|"127.0.0.1"|list.string|
|$.api_definition.version_data.versions.Default.paths.white_list|The value is s string. <br/> It is an IP address|"127.0.0.2"|list.string|
|$.api_definition.version_data.versions.Default.paths.black_list|The value is s string. <br/> It is an IP address|"127.0.0.3"|list.string|
|$.api_definition.version_data.versions.Default.extended_paths.ignored|Follow the instructions mentioned above this table. The value provided here is just for illustration. Refer the example section for TYK JSON structure.|"{<br/>\\"path\\":\\"/test/reply1\\",<br/>\\"description\\":\\"Testing\\"<br/>}"|list|
|$.api_definition.version_data.versions.Default.extended_paths.white_list|The value is s string. <br/> It is an IP address|"127.0.0.4"|list.string|
|$.api_definition.version_data.versions.Default.extended_paths.black_list|The value is s string. <br/> It is an IP address|"127.0.0.5"|list.string|
|$.api_definition.oauth_meta.allowed_access_types|The value is s string.|"token2"|list.string|
|$.api_definition.oauth_meta.allowed_authorize_types|The value is s string.|"token1"|list.string|
|$.api_definition.allowed_ips|The value is s string. <br/> It is an IP address|"127.0.0.6"|list.string|
|$.hook_references|Follow the instructions mentioned above this table. The value provided here is just for illustration. Refer the example section for TYK JSON structure.|"{<br/>\\"event_name\\":\\"Quota\\",<br/>\\"description\\": \\"Quota\\"<br/>}"|list|

### 3.Updating a particular element in JSONArray

A few examples for updating a particular element in JSONArray are given here. This can be implemented on any JSONArray whose path is mentioned in the above table(`Adding an element into JSONArr`). Here the index of the element to be updated should also be mentioned.

| path| Explanation of the instruction| value | type |  
| :----| :-----------------------------|:-----|:----:|
|$.api_definition.version_data.versions.Default.extended_paths.ignored[1]|Updating the element indexed 1. Since this array contains JSONObject, the value should be a stringified JSONObject|"{<br/>\\"path\\":\\"/test-path/reply\\",<br/>\\"description\\":\\"Testing\\"<br/>}"|object|
|$.api_definition.oauth_meta.allowed_authorize_types[0]|Updating the element indexed 0 | "TOKEN"| `NA` |
|$.api_definition.allowed_ips[0]|Updating the element indexed 1 | "127.0.0.4"| `NA` |
|$.hook_references[0]|Updating the element indexed 0. Since this array contains JSONObject, the value should be a stringified JSONObject|"{<br/>\\"event_name\\":\\"QuotaExceeded\\",<br/>\\"description\\": \\"Quota\\"<br/>}"|object|

### 4.Adding new field to a JSONObject

To add a new field in any JSONObject, including the JSONOBjects inside JSONArray, the `value` should contain the entire JSON to be modified.

| path| Explanation of the instruction| value | type |  
| :----| :-----------------------------|:-----|:----:|
|$.api_definition.notifications | To add a field ID in notifications JSONObject | "{<br/> \\"ID\\" : 1, <br/> \\"shared_secret\\":\\"\\", <br/> \\"oauth_on_keychange_url\\": \\"\\" <br/>}" | object |
|$.hook_references[0].hook.header_map | To add a field in header_map in the 0 indexed element in hook_references JSONArray | "{ <br/> \\"x-tyk-test\\": \\"123456\\" <br/> \\"y-tyk-test\\":\\"1234\\" <br/> }" | object |


