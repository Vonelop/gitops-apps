# gitops-apps

## GitLab ArgoCD

1. Создай секрет для статических типов данных при помощи команды
- Создай файл object_storage.yaml
provider: AWS
region: ru-moscow-1
aws_access_key_id: 
aws_secret_access_key: 
endpoint: https://obs.ru-moscow-1.hc.sbercloud.ru
path_style: false

- Создай секрет
kubectl create secret generic -n gitlab gitlab-object-storage --from-file=connection=object_storage.yaml


2. Создай секрет для подключения работы ssl
- Скачай с рег.ру ключ и сертификат (certificate.crt, certificate.key)

- Создай секрет
kubectl create secret tls gitlab-tls --cert=certificate.crt --key=certificate.key -n gitlab


3. Создай секрет для востоновления бэкапа
- Сохрани секреты GitLab (gitlab-secrets.yaml)

- Создай секрет
kubectl create secret generic gitlab-backup --from-file=secrets.yml=gitlab-secrets.yaml -n gitlab


4. Создай секрет для подключения сервисов к s3 (s3cfg)
- Сохрони конфигурацию для доступа к s3

- Создай секрет
kubectl create secret generic gitlab-s3cfg-backup --from-file=config=.s3cfg -n gitlab

Поднять приложение AgroCd
kubectl apply -g root-prod.yaml

Восстоновить бэкап внутри пода toolbox
backup-utility --restore -t 1775512833_2026_04_06_18.9.0-ee