### Context
GCP VM에 n8n을 셀프 호스팅하여 구축하는 과정에서 Caddy를 사용하여 HTTPS 인증서를 발급하려 했습니다. DuckDNS 도메인(`chutzrit.duckdns.org`)을 통해 Let's Encrypt 인증서를 발급받으려 하였으나 여러 차례 시도에도 불구하고 실패하였습니다.

### Core
오류 메시지는 다음과 같습니다:

```
DNS problem: SERVFAIL looking up CAA for duckdns.org
- the domain's nameservers may be malfunctioning
```
Let's Encrypt가 인증서 발급 전 `duckdns.org`의 CAA 레코드를 조회하는 과정에서, DuckDNS 네임서버가 간헐적으로 SERVFAIL을 반환하며 실패합니다. ZeroSSL을 사용해도 같은 문제가 발생하였습니다. VM의 DNS를 `1.1.1.1`로 변경하여 문제를 회피할 수 있다는 방법이 있었지만, 이는 근본적인 해결책이 아니었습니다.

최종적으로, Caddy 대신 Ngrok을 선택하여 문제를 해결하였습니다. Ngrok은 도메인, DNS, SSL 문제를 한꺼번에 해결하고, VM 방화벽 설정이 필요없어 편리했습니다.

### Insight
DuckDNS는 무료 DDNS 서비스로, 네임서버 안정성이 충분히 보장되지 않습니다. 개인 테스트 용도로는 적합할 수 있으나, 일관성이 중요한 환경에서는 부적합할 수 있습니다. 이에 비해 Ngrok은 기존의 도메인 관리와 SSL 설정 문제를 동시에 해결할 수 있으며, 셀프호스팅 입문자에게 훨씬 적합한 솔루션임을 확인했습니다.

**출처:** [Let's Encrypt Community](https://community.letsencrypt.org/)