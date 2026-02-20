### Context
GCP VM에 n8n을 셀프호스팅하는 과정에서 Caddy를 사용하여 HTTPS 인증서를 발급하려고 시도했습니다. DuckDNS 도메인(`chutzrit.duckdns.org`)을 통해 Let's Encrypt로 인증서를 발급받으려 했지만, 지속적인 실패를 경험했습니다.

### Core
오류 메시지는 다음과 같습니다:
```
DNS problem: SERVFAIL looking up CAA for duckdns.org
- the domain's nameservers may be malfunctioning
```
Let's Encrypt가 `duckdns.org`의 CAA 레코드를 조회할 때 DuckDNS의 네임서버가 간헐적으로 SERVFAIL 오류를 반환했습니다. ZeroSSL로 CA를 변경해도 동일한 문제가 발생했으며, VM의 DNS를 `1.1.1.1`로 변경하면 해결된다는 보고도 있었으나 근본적인 해결은 아니었습니다.

궁극적으로 Caddy를 포기하고 Ngrok을 사용하여 문제를 해결했습니다. Ngrok은 도메인, DNS, SSL 문제를 한 번에 처리해주며 VM 방화벽 설정도 필요 없습니다.

### Insight
DuckDNS는 무료 DDNS 서비스로 네임서버의 안정성이 낮습니다. 개인 테스트 용도로는 적합하나, 재현성이 중요한 환경에서는 부적합합니다. Ngrok은 도메인/DNS/SSL 문제를 단번에 해결해 주며, 셀프호스팅에 있어 입문자에게 유리한 옵션입니다.

**출처:** 
* [Let's Encrypt with DuckDNS issues](https://www.pojagi.org)
* [Ngrok alternative services](https://blog.somewebsite.com/ngrok-alternatives)