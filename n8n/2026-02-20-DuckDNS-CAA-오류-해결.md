### Context
n8n을 GCP VM에 셀프호스팅하여 구축하는 과정 중, DuckDNS와 Let's Encrypt를 이용해 HTTPS 인증서를 발급받으려 했습니다. DuckDNS 도메인 `chutzrit.duckdns.org`를 사용하여 Caddy를 통해 Let's Encrypt 인증서를 발급받으려고 했으나, 지속적인 CAA 오류로 중단되었습니다.

### Core
오류 메시지는 다음과 같습니다:
```
DNS problem: SERVFAIL looking up CAA for duckdns.org - the domain's nameservers may be malfunctioning
```
Let's Encrypt는 인증서 발급을 위해 `duckdns.org`의 CAA 레코드를 조회하지만, DuckDNS 네임서버가 간헐적으로 SERVFAIL을 반환했습니다. ZeroSSL로 CA를 변경해도 문제가 해결되지 않았으며, VM의 DNS를 `1.1.1.1`로 변경하면 해결된다는 일부 정보가 있었으나 근본적인 문제를 해결하지 못했습니다.

결론적으로 Caddy를 포기하고 **Ngrok**을 사용하여 문제를 해결했습니다.

### Insight
DuckDNS는 무료 DDNS 서비스이기 때문에 네임서버의 안정성이 낮을 수 있습니다. 개인적인 테스트에는 적합할 수 있지만, 재현성이 중요한 환경에는 적절하지 않습니다. 반면 Ngrok은 도메인, DNS, SSL 문제를 한 번에 해결해주고, VM 방화벽 설정도 필요하지 않기 때문에 셀프호스팅을 처음 시도하는 사용자에게 더 적합한 솔루션입니다.

**출처:** [Link1](https://community.letsencrypt.org/t/caa-servfail/8618), [Link2](https://ngrok.com/docs)