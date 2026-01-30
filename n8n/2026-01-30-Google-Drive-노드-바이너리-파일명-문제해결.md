### Context
n8n에서 Google Drive 노드를 사용하여 파일을 업로드할 때, 파일명이 'undefined'로 들어가는 문제가 발생했습니다. 이는 노드 설정의 'Property Name'이 바이너리 데이터의 키값과 일치하지 않을 때 발생하는 이슈입니다.

### Core
```javascript
{
  "nodes": [
    {
      "parameters": {
        "file": {
          "binaryPropertyName": "data"
        },
      },
      "name": "Google Drive"
    }
  ]
}
```
* 설정에서 'Property Name'을 이전 노드에서 정의한 바이너리 키값(e.g., 'data')와 정확히 맞춰야 합니다.
* 이 설정이 맞지 않으면 파일 내용은 전송되지만, 파일명이 'undefined'로 표시됩니다.

### Insight
바이너리 데이터를 Google Drive에 업로드하는 과정에서 'Property Name'을 일치시키는 것이 중요합니다. 이 문제는 바이너리 데이터가 올바르게 전달됨에도 불구하고 파일명이 손상되는 주된 원인으로, 교육이나 강의 시 '바이너리 프로퍼티 이름 일치'의 중요성을 강조해야 합니다.

**출처:** [n8n Google Drive Node Binary File Upload Issue](https://community.n8n.io/t/google-drive-binary-file-upload-issue/8827)