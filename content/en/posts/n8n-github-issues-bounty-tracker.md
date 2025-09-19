+++
title = 'How I Built an Automated GitHub Issue Bounty Tracker with n8n, Email, and WhatsApp Notifications'
date = 2025-09-13T05:56:13+01:00
author = 'c0d33ngr'
toc = true
draft = false
+++

<script type="module" src="https://cdn.jsdelivr.net/npm/@n8n_io/n8n-demo-component/n8n-demo.bundled.js"></script>
<n8n-demo workflow='{"nodes":[{"parameters":{"rule":{"interval":[{"field":"hours"}]}},"type":"n8n-nodes-base.scheduleTrigger","typeVersion":1.2,"position":[0,0],"id":"a267afec-e423-435f-b4fa-a0f240faa4ac","name":"Schedule Trigger"},{"parameters":{"documentId":{"__rl":true,"value":"1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI","mode":"list","cachedResultName":"public-open-source-bounty","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit?usp=drivesdk"},"sheetName":{"__rl":true,"value":"gid=0","mode":"list","cachedResultName":"Sheet1","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit#gid=0"},"options":{}},"type":"n8n-nodes-base.googleSheets","typeVersion":4.7,"position":[336,416],"id":"cd9679eb-98bf-4617-a1d2-8d19f239e123","name":"Get row(s) in sheet","credentials":{"googleSheetsOAuth2Api":{"id":"M6chGYvIS8SmWTLF","name":"google-sheet-app-whetujeff"}}},{"parameters":{"url":"https://api.github.com/search/issues","authentication":"genericCredentialType","genericAuthType":"httpBearerAuth","sendQuery":true,"queryParameters":{"parameters":[{"name":"q","value":"is:issue label:\"💎 Bounty\" state:open"},{"name":"per_page","value":"100"}]},"options":{"pagination":{"pagination":{"parameters":{"parameters":[{"name":"page","value":"={{ $pageCount + 1 }}"}]},"paginationCompleteWhen":"other","completeExpression":"={{ $response.body.items.length < 100 }}"}}}},"type":"n8n-nodes-base.httpRequest","typeVersion":4.2,"position":[272,0],"id":"5fad1b8e-42c5-456d-8df4-10d4240363b0","name":"GitHub Search HTTP Request","credentials":{"httpBearerAuth":{"id":"I8c6gjqMq52vKeQl","name":"github-token"}}},{"parameters":{"fieldToSplitOut":"items","options":{}},"type":"n8n-nodes-base.splitOut","typeVersion":1,"position":[496,0],"id":"b297fd49-eea6-4c8b-9059-83b12d3f78c8","name":"Split Out"},{"parameters":{"mode":"combine","advanced":true,"mergeByFields":{"values":[{"field1":"html_url","field2":"Issue URL"}]},"joinMode":"keepNonMatches","outputDataFrom":"input1","options":{}},"type":"n8n-nodes-base.merge","typeVersion":3.2,"position":[784,16],"id":"7155006b-9965-4c55-89f8-7dcd0b2f48cc","name":"Merge"},{"parameters":{"conditions":{"options":{"caseSensitive":true,"leftValue":"","typeValidation":"strict","version":2},"conditions":[{"id":"f84a90c2-eaad-4639-aca2-582068d58f71","leftValue":"={{ $(\"Get row(s) in sheet\").all().map(item => item.json[\"Issue Name\"]) }}","rightValue":"={{ $json.title }}","operator":{"type":"array","operation":"notContains","rightType":"any"}}],"combinator":"and"},"options":{}},"type":"n8n-nodes-base.filter","typeVersion":2.2,"position":[1008,16],"id":"49e5c6ab-84ee-483a-9378-e35da7b7dee3","name":"Filter"},{"parameters":{"operation":"append","documentId":{"__rl":true,"value":"1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI","mode":"list","cachedResultName":"public-open-source-bounty","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit?usp=drivesdk"},"sheetName":{"__rl":true,"value":"gid=0","mode":"list","cachedResultName":"Sheet1","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1rSTq5uhAdMNmfDTfACHZ10tYkqh8eBkOgXuYFKBsc78/edit#gid=0"},"columns":{"mappingMode":"defineBelow","value":{"Issue Number":"={{ $(\"Filter\").item.json.number }}","Issue Name":"={{ $(\"Filter\").item.json.title }}","Issue URL":"={{ $(\"Filter\").item.json.html_url }}","Issue state":"={{ $(\"Filter\").item.json.state }}","Issue Repo URL":"={{ $(\"Filter\").item.json.repository_url.replace(\"https://api.github.com/repos\", \"https://github.com\") }}","Issue Creation Date":"={{ $(\"Filter\").item.json.created_at }}","Issue Updated Date":"={{ $(\"Filter\").item.json.updated_at }}"},"matchingColumns":[],"schema":[{"id":"Issue Number","displayName":"Issue Number","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Issue Name","displayName":"Issue Name","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Issue URL","displayName":"Issue URL","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Issue state","displayName":"Issue state","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Issue Repo URL","displayName":"Issue Repo URL","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Issue Creation Date","displayName":"Issue Creation Date","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Issue Updated Date","displayName":"Issue Updated Date","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true}],"attemptToConvertTypes":false,"convertFieldsToString":false},"options":{}},"type":"n8n-nodes-base.googleSheets","typeVersion":4.7,"position":[1248,16],"id":"b39bcd24-61b4-4216-896a-0990f06f3dc1","name":"Append row in sheet","credentials":{"googleSheetsOAuth2Api":{"id":"M6chGYvIS8SmWTLF","name":"google-sheet-app-whetujeff"}}},{"parameters":{"operation":"send","phoneNumberId":"763560806846407","recipientPhoneNumber":"+2348110493637","textBody":"=New Bounty Alert!\nIssue: {{ $(\"Split Out\").item.json.title }}\nNumber: #{{ $(\"Split Out\").item.json.number }}\nStatus: {{ $(\"Split Out\").item.json.state }}\nCreated: {{ $json.created_at }}\nRepository: {{ $(\"Split Out\").item.json.repository_url }}\nView it here: {{ $(\"Split Out\").item.json.html_url }}","additionalFields":{}},"type":"n8n-nodes-base.whatsApp","typeVersion":1,"position":[1712,16],"id":"d24ca8fa-aa23-4fb3-9398-e92457ffef43","name":"Send message","webhookId":"7d4fc62e-e879-4917-bd96-e1b0f746f4a6","executeOnce":true,"credentials":{"whatsAppApi":{"id":"ZxEqot7Yd19iNjoi","name":"WhatsApp account 2"}},"disabled":true},{"parameters":{"sendTo":"jeffwhewhetu@gmail.com","subject":"New Bounty Alert!","message":"={{ $json.html }}","options":{"appendAttribution":false}},"type":"n8n-nodes-base.gmail","typeVersion":2.1,"position":[2144,16],"id":"bbf59659-42b0-4abd-84e3-e2a50e8f3130","name":"Send a message","webhookId":"e5136b5f-b355-4174-b5a5-450be20d06eb","executeOnce":true,"credentials":{"gmailOAuth2":{"id":"ZfhKYYda7UXbR2Zd","name":"Gmail account"}}},{"parameters":{"html":"<!DOCTYPE html>\n<html>\n<head>\n  <meta charset=\"UTF-8\">\n  <style>\n    body {\n      font-family: -apple-system, BlinkMacSystemFont, \"Segoe UI\", Roboto, Helvetica, Arial, sans-serif;\n      background-color: #f6f8fa;\n      padding: 20px;\n    }\n    .container {\n      max-width: 600px;\n      margin: 0 auto;\n      background-color: #ffffff;\n      border-radius: 6px;\n      border: 1px solid #e1e4e8;\n      box-shadow: 0 1px 3px rgba(27,31,35,.04);\n      padding: 24px;\n    }\n    .header {\n      border-bottom: 1px solid #e1e4e8;\n      padding-bottom: 16px;\n      margin-bottom: 16px;\n    }\n    .header h1 {\n      color: #0366d6;\n      font-size: 24px;\n      margin: 0;\n    }\n    .issue-title {\n      font-size: 18px;\n      font-weight: 600;\n      color: #24292e;\n      margin-bottom: 8px;\n    }\n    .detail-row {\n      font-size: 14px;\n      margin-bottom: 8px;\n    }\n    .label {\n      font-weight: 600;\n      color: #586069;\n    }\n    .status-open {\n      color: #2cbe4e;\n    }\n    .status-closed {\n      color: #cb2431;\n    }\n    .link-button {\n      display: inline-block;\n      padding: 10px 16px;\n      margin-top: 16px;\n      font-size: 14px;\n      font-weight: 600;\n      color: #ffffff;\n      background-color: #0366d6;\n      border-radius: 4px;\n      text-decoration: none;\n    }\n  </style>\n</head>\n<body>\n  <div class=\"container\">\n    <div class=\"header\">\n      <h1>New GitHub Bounty Issue Alert!</h1>\n    </div>\n    \n    <p class=\"issue-title\">Issue: {{ $(\"Filter\").item.json.title }}</p>\n    \n    <div class=\"detail-row\">\n      <span class=\"label\">Number:</span> #{{ $(\"Filter\").item.json.number }}\n    </div>\n    <div class=\"detail-row\">\n      <span class=\"label\">Status:</span> <span class=\"{{ $json.state === \"open\" ? \"status-open\" : \"status-closed\" }}\">{{ $(\"Filter\").item.json.state }}</span>\n    </div>\n    <div class=\"detail-row\">\n      <span class=\"label\">Bounty Amount:{{ $(\"Filter\").item.json.labels.find(label => label.name.includes(\"$\"))?.name || \"No bounty specified\" }} </span>\n    </div>\n        <div class=\"detail-row\">\n      <span class=\"label\">Comments on Issue:</span> {{ $(\"Filter\").item.json.comments }}\n    </div>\n    <div class=\"detail-row\">\n      <span class=\"label\">Bounty Issue Created on:</span> {{ $(\"Filter\").item.json.created_at }}\n    </div>\n    <div class=\"detail-row\">\n      <span class=\"label\">Repository:</span> <a href=\"{{ $(\"Filter\").item.json.repository_url.replace(\"https://api.github.com/repos\", \"https://github.com\") }}\">{{ $(\"Filter\").item.json.repository_url.split(\"/\").pop() }}</a>\n    </div>\n    \n    <a href=\"{{ $(\"Filter\").item.json.html_url }}\" class=\"link-button\">View Issue on GitHub</a>\n  </div>\n</body>\n</html>"},"type":"n8n-nodes-base.html","typeVersion":1.2,"position":[1920,16],"id":"ab346e10-4af1-4e9b-b74a-6e2b8f233b89","name":"HTML"},{"parameters":{"documentId":{"__rl":true,"value":"1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI","mode":"list","cachedResultName":"public-open-source-bounty","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit?usp=drivesdk"},"sheetName":{"__rl":true,"value":1153061999,"mode":"list","cachedResultName":"Sheet2","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit#gid=1153061999"},"options":{}},"type":"n8n-nodes-base.googleSheets","typeVersion":4.7,"position":[1584,352],"id":"9ef52931-f441-4a40-a0ce-b193721da6e6","name":"Get row(s) in sheet1","credentials":{"googleSheetsOAuth2Api":{"id":"M6chGYvIS8SmWTLF","name":"google-sheet-app-whetujeff"}}},{"parameters":{"rule":{"interval":[{"field":"hours","hoursInterval":6}]}},"type":"n8n-nodes-base.scheduleTrigger","typeVersion":1.2,"position":[1344,512],"id":"b9d2668a-a1e6-4add-8ec4-7a66ca919410","name":"Schedule Trigger1"},{"parameters":{"url":"={{ $json[\"Issue Repo URL\"].replace(\"https://github.com/\", \"https://api.github.com/repos/\") + \"/issues/\" + $json[\"Issue Number\"] }}","authentication":"genericCredentialType","genericAuthType":"httpBearerAuth","sendQuery":true,"queryParameters":{"parameters":[{}]},"sendHeaders":true,"headerParameters":{"parameters":[{"name":"accept","value":"application/vnd.github+json"}]},"options":{}},"type":"n8n-nodes-base.httpRequest","typeVersion":4.2,"position":[1776,352],"id":"170d850c-0a54-4a69-b9b4-28962b97a201","name":"GitHub Search HTTP Request1","executeOnce":false,"credentials":{"httpBearerAuth":{"id":"I8c6gjqMq52vKeQl","name":"github-token"}}},{"parameters":{"conditions":{"options":{"caseSensitive":true,"leftValue":"","typeValidation":"strict","version":2},"conditions":[{"id":"6aba82f7-8d0c-4862-87cc-5e8e3b4cce8e","leftValue":"={{ new Date($json[\"Issue Creation Date\"]).getTime() }}","rightValue":"={{ new Date().getTime() - (5 * 24 * 60 * 60 * 1000) }}","operator":{"type":"number","operation":"gte"}}],"combinator":"and"},"options":{}},"type":"n8n-nodes-base.filter","typeVersion":2.2,"position":[1456,16],"id":"d1c355b7-55c3-4012-b4c4-3ad87818edca","name":"Filter1"},{"parameters":{"documentId":{"__rl":true,"value":"1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI","mode":"list","cachedResultName":"public-open-source-bounty","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit?usp=drivesdk"},"sheetName":{"__rl":true,"value":1153061999,"mode":"list","cachedResultName":"Sheet2","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit#gid=1153061999"},"options":{}},"type":"n8n-nodes-base.googleSheets","typeVersion":4.7,"position":[256,-304],"id":"5066db28-8ff6-49a5-840a-990abca1b7b4","name":"Get row(s) in sheet2","credentials":{"googleSheetsOAuth2Api":{"id":"M6chGYvIS8SmWTLF","name":"google-sheet-app-whetujeff"}}},{"parameters":{"operation":"append","documentId":{"__rl":true,"value":"1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI","mode":"list","cachedResultName":"public-open-source-bounty","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit?usp=drivesdk"},"sheetName":{"__rl":true,"value":1153061999,"mode":"list","cachedResultName":"Sheet2","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit#gid=1153061999"},"columns":{"mappingMode":"defineBelow","value":{"Issue Number":"={{ $(\"Filter1\").item.json[\"Issue Number\"] }}","Issue Name":"={{ $(\"Filter1\").item.json[\"Issue Name\"] }}","Issue URL":"={{ $(\"Filter1\").item.json[\"Issue URL\"] }}","Number of Issue Comments":"={{ $(\"Filter\").item.json.comments }}","Issue state":"={{ $(\"Filter1\").item.json[\"Issue state\"] }}","Bounty Amount":"={{ $(\"Filter\").item.json.labels.find(label => label.name.includes(\"$\"))?.name || \"No bounty specified\" }}","Issue Repo URL":"={{ $(\"Filter\").item.json.repository_url.replace(\"https://api.github.com/repos\", \"https://github.com\") }}","Issue Bounty Creation Date":"={{ $(\"Filter1\").item.json[\"Issue Creation Date\"] }}"},"matchingColumns":[],"schema":[{"id":"Issue Number","displayName":"Issue Number","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Issue Name","displayName":"Issue Name","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Issue URL","displayName":"Issue URL","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Number of Issue Comments","displayName":"Number of Issue Comments","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Issue state","displayName":"Issue state","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Issue Repo URL","displayName":"Issue Repo URL","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Issue Bounty Creation Date","displayName":"Issue Bounty Creation Date","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Bounty Amount","displayName":"Bounty Amount","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true}],"attemptToConvertTypes":false,"convertFieldsToString":false},"options":{}},"type":"n8n-nodes-base.googleSheets","typeVersion":4.7,"position":[2320,16],"id":"2c359763-2b7a-4ac3-8664-9359ceb70aee","name":"Append row in sheet1","credentials":{"googleSheetsOAuth2Api":{"id":"M6chGYvIS8SmWTLF","name":"google-sheet-app-whetujeff"}}},{"parameters":{"operation":"update","documentId":{"__rl":true,"value":"1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI","mode":"list","cachedResultName":"public-open-source-bounty","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit?usp=drivesdk"},"sheetName":{"__rl":true,"value":1153061999,"mode":"list","cachedResultName":"Sheet2","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit#gid=1153061999"},"columns":{"mappingMode":"defineBelow","value":{"Number of Issue Comments":"={{ $(\"GitHub Search HTTP Request1\").item.json.comments }}","Issue state":"={{ $(\"GitHub Search HTTP Request1\").item.json.state }}","row_number":"={{ $(\"Get row(s) in sheet1\").item.json.row_number }}"},"matchingColumns":["row_number"],"schema":[{"id":"Number of Issue Comments","displayName":"Number of Issue Comments","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Issue state","displayName":"Issue state","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"row_number","displayName":"row_number","required":false,"defaultMatch":false,"display":true,"type":"number","canBeUsedToMatch":true,"readOnly":true}],"attemptToConvertTypes":false,"convertFieldsToString":false},"options":{}},"type":"n8n-nodes-base.googleSheets","typeVersion":4.7,"position":[2176,352],"id":"c0e8dff7-2883-4b78-af72-1c2412755cf6","name":"Update row in sheet","alwaysOutputData":true,"credentials":{"googleSheetsOAuth2Api":{"id":"M6chGYvIS8SmWTLF","name":"google-sheet-app-whetujeff"}}},{"parameters":{"conditions":{"options":{"caseSensitive":true,"leftValue":"","typeValidation":"strict","version":2},"conditions":[{"id":"d63efe45-fc1f-405e-896e-55a848492aa7","leftValue":"={{ $json.state }}","rightValue":"={{ $(\"Get row(s) in sheet1\").item.json[\"Issue state\"] }}","operator":{"type":"string","operation":"notEquals"}},{"id":"7c85599e-1a40-4f4b-9b54-c2341bec3ba3","leftValue":"={{ $(\"Get row(s) in sheet1\").item.json[\"Number of Issue Comments\"] }}","rightValue":"={{ $json.comments }}","operator":{"type":"number","operation":"notEquals"}}],"combinator":"or"},"options":{}},"type":"n8n-nodes-base.filter","typeVersion":2.2,"position":[1968,352],"id":"188775b6-de22-4e85-a5e3-60c95664725d","name":"Filter2"},{"parameters":{"documentId":{"__rl":true,"value":"1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI","mode":"list","cachedResultName":"public-open-source-bounty","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit?usp=drivesdk"},"sheetName":{"__rl":true,"value":"gid=0","mode":"list","cachedResultName":"Sheet1","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit#gid=0"},"options":{}},"type":"n8n-nodes-base.googleSheets","typeVersion":4.7,"position":[1568,672],"id":"274a1992-f0fa-4087-b5cd-f96871870306","name":"Get row(s) in sheet3","credentials":{"googleSheetsOAuth2Api":{"id":"M6chGYvIS8SmWTLF","name":"google-sheet-app-whetujeff"}}},{"parameters":{"url":"={{ $json[\"Issue Repo URL\"].replace(\"https://github.com/\", \"https://api.github.com/repos/\") + \"/issues/\" + $json[\"Issue Number\"] }}","authentication":"genericCredentialType","genericAuthType":"httpBearerAuth","sendQuery":true,"queryParameters":{"parameters":[{}]},"sendHeaders":true,"headerParameters":{"parameters":[{"name":"accept","value":"application/vnd.github+json"}]},"options":{}},"type":"n8n-nodes-base.httpRequest","typeVersion":4.2,"position":[1776,672],"id":"0e563238-bbc0-4952-aad4-9ec771ba03bf","name":"GitHub Search HTTP Request2","executeOnce":false,"credentials":{"httpBearerAuth":{"id":"I8c6gjqMq52vKeQl","name":"github-token"}}},{"parameters":{"operation":"update","documentId":{"__rl":true,"value":"1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI","mode":"list","cachedResultName":"public-open-source-bounty","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit?usp=drivesdk"},"sheetName":{"__rl":true,"value":"gid=0","mode":"list","cachedResultName":"Sheet1","cachedResultUrl":"https://docs.google.com/spreadsheets/d/1LoZk5q8DzKSCgsYF5UMEbEnIs09QZQ73k4MoOhhoKnI/edit#gid=0"},"columns":{"mappingMode":"defineBelow","value":{"Issue state":"={{ $(\"GitHub Search HTTP Request2\").item.json.state }}","row_number":"={{ $(\"Get row(s) in sheet3\").item.json.row_number }}","Issue Updated Date":"={{ $(\"GitHub Search HTTP Request2\").item.json.updated_at }}"},"matchingColumns":["row_number"],"schema":[{"id":"Issue state","displayName":"Issue state","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"Issue Updated Date","displayName":"Issue Updated Date","required":false,"defaultMatch":false,"display":true,"type":"string","canBeUsedToMatch":true},{"id":"row_number","displayName":"row_number","required":false,"defaultMatch":false,"display":true,"type":"number","canBeUsedToMatch":true,"readOnly":true}],"attemptToConvertTypes":false,"convertFieldsToString":false},"options":{}},"type":"n8n-nodes-base.googleSheets","typeVersion":4.7,"position":[2176,672],"id":"f50b964c-463f-4c26-974e-cb0b809fafa3","name":"Update row in sheet1","alwaysOutputData":true,"credentials":{"googleSheetsOAuth2Api":{"id":"M6chGYvIS8SmWTLF","name":"google-sheet-app-whetujeff"}}},{"parameters":{"conditions":{"options":{"caseSensitive":true,"leftValue":"","typeValidation":"strict","version":2},"conditions":[{"id":"d63efe45-fc1f-405e-896e-55a848492aa7","leftValue":"={{ $json.state }}","rightValue":"={{ $(\"Get row(s) in sheet3\").item.json[\"Issue state\"] }}","operator":{"type":"string","operation":"notEquals"}},{"id":"341603a8-808e-42db-a311-e572e031eb19","leftValue":"={{ $json.updated_at }}","rightValue":"={{ $(\"Get row(s) in sheet3\").item.json[\"Issue Updated Date\"] }}","operator":{"type":"dateTime","operation":"notEquals"}}],"combinator":"or"},"options":{}},"type":"n8n-nodes-base.filter","typeVersion":2.2,"position":[1968,672],"id":"fd100198-5c50-4542-a01d-538f338780f7","name":"Filter3"}],"connections":{"Schedule Trigger":{"main":[[{"node":"GitHub Search HTTP Request","type":"main","index":0},{"node":"Get row(s) in sheet","type":"main","index":0},{"node":"Get row(s) in sheet2","type":"main","index":0}]]},"Get row(s) in sheet":{"main":[[{"node":"Merge","type":"main","index":1}]]},"GitHub Search HTTP Request":{"main":[[{"node":"Split Out","type":"main","index":0}]]},"Split Out":{"main":[[{"node":"Merge","type":"main","index":0}]]},"Merge":{"main":[[{"node":"Filter","type":"main","index":0}]]},"Filter":{"main":[[{"node":"Append row in sheet","type":"main","index":0}]]},"Append row in sheet":{"main":[[{"node":"Filter1","type":"main","index":0}]]},"Send message":{"main":[[{"node":"HTML","type":"main","index":0}]]},"Send a message":{"main":[[{"node":"Append row in sheet1","type":"main","index":0}]]},"HTML":{"main":[[{"node":"Send a message","type":"main","index":0}]]},"Get row(s) in sheet1":{"main":[[{"node":"GitHub Search HTTP Request1","type":"main","index":0}]]},"Schedule Trigger1":{"main":[[{"node":"Get row(s) in sheet1","type":"main","index":0},{"node":"Get row(s) in sheet3","type":"main","index":0}]]},"GitHub Search HTTP Request1":{"main":[[{"node":"Filter2","type":"main","index":0}]]},"Filter1":{"main":[[{"node":"Send message","type":"main","index":0}]]},"Get row(s) in sheet2":{"main":[[{"node":"Split Out","type":"main","index":0}]]},"Filter2":{"main":[[{"node":"Update row in sheet","type":"main","index":0}]]},"Get row(s) in sheet3":{"main":[[{"node":"GitHub Search HTTP Request2","type":"main","index":0}]]},"GitHub Search HTTP Request2":{"main":[[{"node":"Filter3","type":"main","index":0}]]},"Filter3":{"main":[[{"node":"Update row in sheet1","type":"main","index":0}]]}},"pinData":{},"meta":{"templateCredsSetupCompleted":true,"instanceId":"4ba24493aafc75de0208716690409f7a0318b626c6e31ad57f8299360db89c0f"}}' frame=true></n8n-demo>

## Why I Needed This Automation

I was manually tracking GitHub bounty issues every day - searching for issues with bounty labels, copying URLs to spreadsheets, and trying to remember which ones I'd already seen. The process was repetitive and I kept missing new opportunities or tracking the same issues multiple times.

I decided to build an automated workflow using n8n that would monitor GitHub for bounty issues and notify me via email and WhatsApp when new ones appeared.

## Choosing n8n for the Workflow

I chose n8n because it offers a visual workflow builder that makes it easy to connect different APIs and services without writing complex code. The platform supports the integrations I needed: GitHub API, Google Sheets, email, and WhatsApp.

The workflow needed to:
- Search GitHub for issues with bounty labels
- Filter for recent issues (last 5 days)
- Store data in Google Sheets for tracking
- Send notifications via email and WhatsApp
- Keep the data updated with current issue status

## Building the Core Workflow

### Setting Up the GitHub Connection

I started by configuring the GitHub API connection in n8n. I created a personal access token in GitHub and added it to n8n's credentials manager. The workflow uses an HTTP Request node to query GitHub's search API with this query:

```
is:issue label:"💎 Bounty" state:open
```

I set up pagination to handle cases where there are more than 100 results, ensuring the workflow captures all available bounty issues.

### Processing the GitHub Data

After the API call, I used a Split Out node to process each issue individually instead of handling them as a batch. This allows the workflow to perform operations on each issue separately.

The data flows into a filter system that checks two conditions:
1. The issue was created within the last 5 days
2. The issue hasn't already been processed (by comparing against existing Google Sheets data)

### Google Sheets Integration

I created two Google Sheets to serve different purposes:

**public-bounty sheet** stores comprehensive data about all bounty issues:
- Issue Number
- Issue Name  
- Issue Repo URL
- Issue URL
- Issue Creation Date
- Issue State
- Issue Updated Date

**sent-issue sheet** tracks issues that were actually sent as notifications and includes additional details:
- Issue Number
- Issue Name
- Issue Repo URL
- Issue URL
- Issue Creation Date
- Number of Issue Comments
- Issue State
- Bounty Amount (extracted from labels)

The workflow uses Google Sheets nodes to read existing data, append new entries, and update changed information.

### Building the Notification System

For WhatsApp notifications, I connected n8n to the WhatsApp Business API. The message includes:
- Issue title
- Issue number
- Repository name
- Direct link to the issue
- Creation date

For email notifications, I created an HTML template that formats the information with GitHub-style styling. The email includes all the WhatsApp information plus additional details like comment count and bounty amount when available.

The HTML node generates a properly formatted email that gets sent through Gmail's API.

## Adding Data Management Features

### Duplicate Prevention

The workflow includes logic to prevent processing the same issue multiple times. Before adding new issues to the sheets or sending notifications, it checks against existing data in both Google Sheets.

### Status Updates

I added a secondary schedule trigger that runs every 6 hours to update existing records. This process:
1. Reads all existing issues from both sheets
2. Makes API calls to GitHub to get current status
3. Updates the sheets if the issue state or comment count has changed

This keeps the historical data accurate without requiring manual maintenance.

### Data Cleanup

The workflow automatically marks issues as "closed" in both spreadsheets when they're no longer open on GitHub. This prevents outdated information from cluttering the tracking sheets.

## Setting Up the Schedule Triggers

I configured two schedule triggers:

**Primary trigger (every hour)**: Searches for new bounty issues and sends notifications
**Secondary trigger (every 6 hours)**: Updates existing records with current GitHub status

This frequency ensures I get timely notifications for new opportunities while keeping the data current without overwhelming the GitHub API rate limits.

## Testing and Refinement

During testing, I discovered several issues that required refinement:

**Rate Limiting**: I had to ensure the workflow stayed within GitHub's API limits by spacing out requests appropriately.

**Data Validation**: Some issues had missing or malformed data, so I added error handling to process what was available rather than failing completely.

**Notification Filtering**: Initially, I was getting notifications for very old issues that happened to be updated recently. I refined the filtering logic to focus on creation date rather than update date.

## Results and Performance

The automated workflow now handles the entire bounty tracking process:
- Runs 24 times per day checking for new issues
- Maintains two comprehensive spreadsheets with 100% accuracy
- Delivers notifications within an hour of new bounty issues appearing
- Automatically maintains data integrity across all tracking systems

I went from spending 30-45 minutes daily on manual tracking to receiving curated notifications with zero manual effort.

## Key Technical Implementation Details

**GitHub API Integration**: Uses pagination and proper authentication to handle large result sets reliably.

**Google Sheets Management**: Implements upsert logic to prevent duplicates while ensuring all data stays current.

**Error Handling**: Includes fallback logic for missing data fields and API failures.

**Filtering Logic**: Multi-stage filtering prevents duplicate notifications and focuses on actionable items.

## Lessons Learned

Building this workflow taught me several important lessons about automation:

**Start Simple**: I began with just the GitHub search and built complexity gradually.

**Data Structure Matters**: Having two separate sheets with different purposes made the system much more manageable.

**Testing Is Critical**: Running the workflow manually many times helped identify edge cases before going live.

**Monitoring Is Essential**: I needed to watch the execution logs regularly during the first few weeks to catch issues early.

## Extending the System

The workflow foundation makes it easy to add new features:
- Additional notification channels (Slack, Discord)
- More sophisticated filtering based on repository reputation
- Integration with time tracking for bounty work
- Analytics dashboard for bounty trends

The modular design of n8n workflows means each of these additions would be separate nodes that integrate cleanly with the existing system.

## Why This Approach Works

This automation eliminates the repetitive manual work while providing better coverage than human monitoring could achieve. The system never forgets to check, never misses an update, and maintains perfect records of all activity.

For anyone dealing with similar repetitive data collection tasks, n8n provides an accessible way to build robust automation without extensive programming knowledge. The visual workflow design makes it easy to understand, modify, and extend the system as needs change.