### Context
GCP VM에 n8n을 셀프호스팅으로 구축 중이었습니다. HTTPS를 위해 Caddy를 사용하여 Let's Encrypt 인증서를 발급받으려 했으나, DuckDNS 도메인(`chutzrit.duckdns.org`)으로 시도할 때 문제가 발생했습니다. "DNS problem: SERVFAIL looking up CAA for duckdns.org"와 같은 오류가 지속되었습니다. ZeroSSL로도 시도했지만 동일한 문제가 나타났습니다.

### Core
DuckDNS는 네임서버의 간헐적 오류로 CAA 레코드 조회에 실패하고 있으며, 이는 Let's Encrypt와 ZeroSSL 모두에서 문제를 일으킵니다. 문제 해결법으로 VM의 DNS 설정을 `1.1.1.1`로 변경하는 방법이 있었으나, 이는 근본적인 해결책이 아니었습니다.

```plaintext
DNS problem: SERVFAIL looking up CAA for duckdns.org
- the domain's nameservers may be malfunctioning
```

해결을 위해 Caddy를 포기하고 Ngrok을 사용했습니다. Ngrok은 SSL 인증 및 도메인 문제를 자동으로 해결하며 방화벽 설정 없이도 셀프호스팅 환경에 적합합니다.

### Insight
DuckDNS는 무료 서비스로 안정성을 보장하지 않기에, 이는 개인 프로젝트에서는 적절할 수 있으나 재현성이 중요한 프로젝트에는 적합하지 않습니다. Ngrok은 설정이 간편하여 셀프호스팅 입문에 유리합니다. 

**출처:** [Let's Encrypt DNS problem solutions](https://community.letsencrypt.org) [Caddy and Ngrok Alternatives](https://discourse.caddyserver.com)