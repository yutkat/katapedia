# Redmineメール通知

configuration.yml全部コメント後


~~~
production:
email_delivery:
delivery_method: :smtp
smtp_settings:
address: mail_server_ip
port: 25
domain: domain_ip
authentication: :login
user_name: mail@address
password: xxxxx

~~~

