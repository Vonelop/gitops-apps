# gitops-apps

GitLab ArgoCD

Создать K8s secret gitlab-s3-creds
kubectl create secret generic gitlab-s3 -n gitlab --from-literal=connection='{
    "provider": "AWS",
    "endpoint": "https://obs.ru-moscow-1.hc.sbercloud.ru",
    "region": "ru-moscow-1",
    "aws_access_key_id": "XBGN2J0FDWD52ODSO3NK",
    "aws_secret_access_key": "I3JCDCiAq0dpoo12sapyGlkPjRI0dIps1ju4Io61", "path_style": false
  }'

Создать K8s secret wildcard-ailabtools-tls
kubectl create secret tls gitlab-ssl \
  --cert=./certificate.crt \
  --key=./certificate.key \
  -n gitlab

Создать секрет для бекапа
kubectl create secret generic gitlab-backup --from-file=secrets.yml=gitlab-secrets.yaml -n gitlab

Создать секрет для подключения к s3 - s3cfg
kubectl create secret generic gitlab-s3cfg-backup --from-file=config=.s3cfg -n gitlab

Поднять приложение AgroCd
	kubectl apply -g root-prod.yaml

Восстоновить бэкап внутри пода toolbox
backup-utility --restore -t 1775512833_2026_04_06_18.9.0-ee