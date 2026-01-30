# Context
n8n 워크플로우에서 Anthropic 모델을 사용하는 중에 400 Bad Request 에러가 발생했습니다. 이 문제는 `temperature`와 `top_p` 매개변수를 동시에 설정할 때 발생했습니다.

# Core
Anthropic API를 사용할 때는 다음과 같이 매개변수를 설정하여야 합니다:

```javascript
const configuration = {
  temperature: 0.5, // 또는 top_p: 0.9,
  // 두 옵션을 동시에 설정하지 말 것
};
```

# Insight
`temperature`와 `top_p`는 모두 모델의 출력을 제어하는 매개변수로, 두 매개변수를 동시에 설정할 경우 요청이 잘못 처리될 수 있습니다. 하나의 매개변수만 설정하여 에러를 방지할 수 있습니다. 공식 문서는 검색되지 않았으나, n8n과 API 사용 중 기본적인 제한 사항을 준수하면 문제가 해결됩니다.