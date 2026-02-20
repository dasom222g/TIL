### Context
n8n 셀프호스팅을 GCP VM에 구축하면서 DuckDNS 도메인(chutzrit.duckdns.org)으로 Caddy를 통해 Let's Encrypt 인증서를 발급받으려 했습니다. 그러나 지속적인 오류가 발생했습니다.

### Core
DuckDNS 사용 시 다음과 같은 오류가 발생합니다:

```
DNS problem: SERVFAIL looking up CAA for duckdns.org - the domain's nameservers may be malfunctioning
```

Let's Encrypt는 인증서 발급 전 CAA 레코드를 확인합니다. DuckDNS의 네임서버가 간헐적으로 SERVFAIL을 반환하며, ZeroSSL로 전환해도 문제는 해결되지 않았습니다.

VM DNS를 1.1.1.1로 설정하는 대안이 있었지만 근본적 해결책은 아니었습니다. Ngrok으로 전환하는 전략을 선택하여 해결했습니다.

### Insight
DuckDNS는 무료 서비스로 인해 네임서버의 안정성이 부족해 신뢰할 수 없는 경우가 있습니다. 개인 테스트에는 적합하나, 영상 콘텐츠 등 재현성이 중요한 환경에서는 부적합할 수 있습니다. Ngrok은 도메인, DNS, SSL을 간편하게 관리할 수 있으며, VM의 방화벽 설정도 간소화할 수 있어 셀프호스팅 초보자에게 더 적합합니다.

**출처:** [Caddy Ngrok setup for self-hosted n8n](https://example.com), [Why DuckDNS has SERVFAIL issues](https://example.com)