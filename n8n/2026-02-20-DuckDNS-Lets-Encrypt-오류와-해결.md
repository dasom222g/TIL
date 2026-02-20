### Context
GCP VM 상에서 n8n 셀프호스팅을 구축하는 과정에서 DuckDNS와 Let's Encrypt를 사용하여 HTTPS 인증서를 발급 받으려 했으나, CAA 레코드 조회 오류로 인해 실패하였다. 이 과정에서 다른 인증 기관인 ZeroSSL로 변경해도 문제가 지속되었다.

### Core
GCP VM에서 n8n을 배포할 때, DNS를 DuckDNS로 설정하고 Caddy를 통해 HTTPS 인증서를 발급받는 과정에서 다음과 같은 오류가 발생했다:

```
DNS problem: SERVFAIL looking up CAA for duckdns.org
- the domain's nameservers may be malfunctioning
```

이 문제는 DuckDNS의 네임서버 안정성 부족으로 인해 발생했으며, 네임서버가 간헐적으로 SERVFAIL을 반환하는 것이 원인이었다. 근본적인 해결책은 아니지만, VM의 DNS를 `1.1.1.1`로 변경하면 개선될 수 있다. 최종적으로는 Caddy 대신 Ngrok을 사용하여 문제를 해결하였다.

### Insight
DuckDNS는 무료 서비스로 네임서버의 안정성이 떨어지며, 인증서 발급 과정에서 문제가 발생할 수 있다. Ngrok는 DNS, SSL, 포트 포워딩 문제를 한 번에 해결해주어 셀프호스팅 초기 단계에 더 적합한 선택이었다. 특히, 방화벽 설정 없이도 쉽게 설정할 수 있어 개발자와 운영자 모두에게 효율적인 솔루션이라고 평가할 수 있다.

**출처:** [n8n Docs](https://docs.n8n.io/getting-started/installation/), [Caddy Documentation](https://caddyserver.com/docs/automatic-https)