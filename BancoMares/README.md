# Training Guide PDF

Download the training guide PDF [here](https://github.com/kkneomis/kc7_data/raw/main/BancoMares/KC7%20Training%20Guide%20-%20Banco%20Mar%C3%A9s.pdf)!

# Script To Import Banco Marés

```kusto
.execute database script <| 
.create-merge table ArquivosCriados (timestamp:datetime, hostname:string, sha256:string, path:string, filename:string) 
.create-merge table Autenticacao (timestamp:datetime, hostname:string, src_ip:string, user_agent:string, username:string, result:string, password_hash:string) 
.create-merge table DnsPassivo (timestamp:datetime, ip:string, domain:string) 
.create-merge table Email (timestamp:datetime, sender:string, reply_to:string, recipient:string, subject:string, accepted:bool, link:string) 
.create-merge table Funcionarios (timestamp:datetime, name:string, user_agent:string, ip_addr:string, email_addr:string, company_domain:string, username:string, role:string, hostname:string) 
.create-merge table NavegacaoExterna (timestamp:datetime, ['method']:string, src_ip:string, user_agent:string, url:string) 
.create-merge table NavegacaoInterna (timestamp:datetime, ['method']:string, src_ip:string, user_agent:string, url:string) 
.create-merge table Processos (timestamp:datetime, parent_process_name:string, parent_process_hash:string, process_commandline:string, process_name:string, process_hash:string, hostname:string, username:string) 
// Import data
.ingest into table ArquivosCriados ('https://github.com/kkneomis/kc7_data/blob/main/BancoMares/Data/ArquivosCriados_1_6c2aece3c8404d41a8ef8d6ae7718e23.csv.gz?raw=true') with (ignoreFirstRecord=true)
.ingest into table Autenticacao ('https://github.com/kkneomis/kc7_data/blob/main/BancoMares/Data/Autenticacao_1_1d0e35677b574e83ab12673539431198.csv.gz?raw=true') with (ignoreFirstRecord=true)
.ingest into table DnsPassivo ('https://github.com/kkneomis/kc7_data/blob/main/BancoMares/Data/DnsPassivo_1_ef9ca9188b5a499ba319aac69e47abd2.csv.gz?raw=true') with (ignoreFirstRecord=true)
.ingest into table Email ('https://github.com/kkneomis/kc7_data/blob/main/BancoMares/Data/Email_1_744125e7e5f641c8892656d5cc9ad3b6.csv.gz?raw=true') with (ignoreFirstRecord=true)
.ingest into table Funcionarios ('https://github.com/kkneomis/kc7_data/blob/main/BancoMares/Data/Funcionarios_1_928e2376639549db8112b0d23d9bd9a7.csv.gz?raw=true') with (ignoreFirstRecord=true)
.ingest into table NavegacaoExterna ('https://github.com/kkneomis/kc7_data/blob/main/BancoMares/Data/NavegacaoExterna_1_da30cb61ab3c4539988ccf02aaa4d433.csv.gz?raw=true') with (ignoreFirstRecord=true)
.ingest into table NavegacaoInterna ('https://github.com/kkneomis/kc7_data/blob/main/BancoMares/Data/NavegacaoInterna_1_416daef656394ab7a43c1544344d38f5.csv.gz?raw=true') with (ignoreFirstRecord=true)
.ingest into table Processos ('https://github.com/kkneomis/kc7_data/blob/main/BancoMares/Data/Processos_1_9504f5b772d94b29932895d3a54667b5.csv.gz?raw=true') with (ignoreFirstRecord=true)


 ```