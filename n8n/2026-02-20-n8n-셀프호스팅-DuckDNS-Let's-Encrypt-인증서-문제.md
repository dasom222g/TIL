### Context
n8n을 GCP VM에 셀프호스팅하기 위해 Caddy를 활용해 HTTPS 인증서를 발급받으려 했습니다. 이 과정에서 DuckDNS 도메인(`chutzrit.duckdns.org`)을 사용하여 Let's Encrypt 인증서를 발급받고자 했으나 지속적으로 인증서 발급에 실패하게 되었습니다. 

### Core
주된 오류는 Let's Encrypt가 `duckdns.org` 도메인의 CAA 레코드를 조회하는 과정에서 발생했습니다. 오류 메시지는 아래와 같습니다:
```
DNS problem: SERVFAIL looking up CAA for duckdns.org
- the domain's nameservers may be malfunctioning
```
이 오류는 DuckDNS의 네임서버가 간헐적으로 SERVFAIL 응답을 반환하는 것에 기인합니다. 인증 기관을 ZeroSSL로 변경해도 같은 문제가 발생했습니다. 

이 문제를 임시방편으로 해결하는 한 가지 방법은 VM의 DNS 설정을 `1.1.1.1`로 변경하는 것입니다. 그러나 이는 근본적인 해결책은 되지 않습니다.

결국, 우리는 Caddy를 포기하고 Ngrok을 활용하여 문제를 해결했습니다.

### Insight
DuckDNS는 무료 DDNS 서비스로, 네임서버의 안정성이 보장되지 않습니다. 이는 개인 테스트용으로는 적합하지만, 영상 콘텐츠와 같이 재현성이 중요한 환경에서는 부적합합니다. 반면, Ngrok은 도메인/DNS/SSL 문제를 한 번에 해결하며, VM 방화벽 설정도 불필요해 셀프호스팅 초심자에게 더욱 적합한 솔루션입니다.

**출처:** [DuckDNS CAA Error Troubleshooting](https://letsencrypt.org/docs/caa/)