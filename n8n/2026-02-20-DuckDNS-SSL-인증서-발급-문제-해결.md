### Context
GCP VM에 n8n을 셀프 호스팅으로 구축하는 과정에서 Caddy 서버를 사용해 HTTPS 인증서를 발급하려고 했습니다. DuckDNS 도메인(`chutzrit.duckdns.org`)을 통해 Let's Encrypt 인증서를 시도했으나 DNS 문제로 실패했습니다.

### Core
인증서 발급 과정에서 다음과 같은 오류가 발생했습니다:
```
DNS problem: SERVFAIL looking up CAA for duckdns.org
- the domain's nameservers may be malfunctioning
```
Caddy에서 DuckDNS의 네임서버가 Let's Encrypt의 CAA 레코드 조회 시 SERVFAIL을 간헐적으로 반환했습니다. ZeroSSL을 사용해도 동일한 문제가 발생했으며, VM의 DNS 설정을 `1.1.1.1`로 변경하면 일시적으로 해결되나 근본적인 해결책은 아닙니다.

이후 인증서 발급 및 외부 노출을 위해 Ngrok 서비스를 사용하는 방법으로 변경하여 문제를 해결했습니다. Ngrok은 도메인, DNS, SSL 문제를 한 번에 해결하며 추가적인 방화벽 설정이 필요 없습니다.

### Insight
DuckDNS는 무료 서비스의 한계로 인해 네임서버의 안정성이 떨어집니다. 개인 테스트 용도에 적합하나, 재현성이 중요한 환경에는 부족합니다. Ngrok은 직접적인 연결을 제공하여 안정적인 셀프호스팅 환경을 제공하며, 설정도 간편하여 입문자에게 추천할 만합니다.

**출처:** [ngrok Documentation](https://ngrok.com/docs), [Let's Encrypt Discussions](https://community.letsencrypt.org/)