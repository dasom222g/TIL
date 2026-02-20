### Context
GCP VM에 n8n 셀프호스팅을 구축하면서 Caddy를 사용하여 HTTPS 인증서를 발급 시도하였습니다. 사용한 도메인은 DuckDNS의 `chutzrit.duckdns.org`로, Let's Encrypt를 통해 인증서를 발급받으려 했으나 실패하였습니다.

### Core
```plaintext
DNS problem: SERVFAIL looking up CAA for duckdns.org
- the domain's nameservers may be malfunctioning
```
Let's Encrypt는 인증서 발급 전 `duckdns.org`의 CAA 레코드를 조회하는데 DuckDNS 네임서버가 간헐적으로 SERVFAIL을 반환하여 문제가 발생했습니다. ZeroSSL로 CA를 변경해도 같은 문제가 발생했고, VM DNS를 `1.1.1.1`로 변경하는 방법이 있지만 근본적인 해결책은 되지 않았습니다.

결국 Caddy 대신 Ngrok을 활용하여 문제를 해결했습니다.

### Insight
DuckDNS 서비스는 무료 DDNS로 네임서버의 안정성이 떨어집니다. 개인 테스트용으로는 적합하지만, 안정성이 요구되는 환경에서는 문제를 일으킬 수 있습니다. Ngrok은 도메인, DNS, SSL 문제를 일괄 해결하고 VM 방화벽 설정이 불필요하여 셀프호스팅 초보자에게 적합한 솔루션입니다.

**출처:** [Let's Encrypt Community Support](https://community.letsencrypt.org/)