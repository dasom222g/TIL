# Context
n8n에서 Anthropic의 AI 모델을 사용할 때 400 Bad Request 오류가 발생하는 문제를 해결했다. 문제는 `temperature`와 `top_p` 파라미터를 동시에 설정했을 때 발생하는데, 이 두 파라미터는 동시에 사용할 수 없다는 것을 알게 되었다.

# Core
Anthropic API를 호출할 때, `temperature`와 `top_p`를 동시에 사용하면 400 Bad Request 오류가 발생한다.

```javascript
{
  "temperature": 0.5,
  "top_p": 0.9
}
```

위와 같은 설정은 오류를 유발한다. 둘 중 하나만 설정해야 한다.

```javascript
{
  "temperature": 0.5
}
```

또는

```javascript
{
  "top_p": 0.9
}
```

# Insight
Anthropic 공식 문서에서는 `temperature`와 `top_p` 파라미터가 모두 모델의 랜덤성을 조절하는 데 사용되지만, 동시에 설정할 경우 모델의 예측 결과가 모호해질 수 있어 오류가 발생한다고 설명한다. 이 문제는 두 파라미터 중 하나만 사용하면 해결된다. 이를 통해 n8n에서 Anthropic 모델을 좀 더 안정적으로 사용할 수 있게 되었다.