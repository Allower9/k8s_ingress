# k8s_ingress

##### 1) файлы nginx-deploy.yaml	nginx-service.yaml - можно сразу запустить
`kubectl apply -f nginx-deploy.yaml	nginx-service.yaml `
#####  2) Проверить текущую работоспособность можно через  `kubectl -n nginx-with-svc run -it --rm=true  --image=curlimages/curl --restart=Never -- curl nginx-service `
#####  3) Так как я делаю через яндекс облако, то создадим сервесный аккаунт(1) и ALB Ingress-контроллер(2) 
#####  4) <img width="1166" height="292" alt="image" src="https://github.com/user-attachments/assets/d573fbba-9eca-4e9a-82ef-62dc96b84a24" />

    после в кластере k8s - Для установки ALB Ingress Controller перейдите в консоли управления на вкладку Marketplace:
<img width="2880" height="1520" alt="image" src="https://github.com/user-attachments/assets/6b70a7a1-fa31-482b-9e28-cf692cbf5c54" />

Найдите в поиске ALB Ingress Controller и зайдите в это приложение:
<img width="2880" height="1520" alt="image" src="https://github.com/user-attachments/assets/47923af9-59a3-4e3f-ab89-d46bb0bf6f18" />


#####   Создайте новое пространство имён для установки в него ALB Ingress Controller, например alb-ingress.
#####   Создайте новый ключ для сервисного аккаунта, созданного в пункте 1.
#####   Установите приложение:
 <img width="2880" height="1520" alt="image" src="https://github.com/user-attachments/assets/2153af66-c70b-4e6e-9c8f-35a3c77dcdfb" />
 
#####  после редактируем наш файл ingress -- нам нужно поменять ` ingress.alb.yc.io/subnets: <id-подсети> ` команда смотрит как-раз id подсети
`yc managed-kubernetes cluster list-node-groups \
    --name <cluster_name> \
    --format json | jq '.[].node_template.network_interface_specs[].subnet_ids[]'` 
#####  and  домен в двух местах 

`- hosts:
        - "allower.online" 
        rules:
    - host: "allower.online"
` 
#####  and id сертификата TLS, SSL  см. фото ниже `secretName: "yc-certmgr-cert-id-fpqvvu8eleba85cg209g" `
<img width="1052" height="202" alt="image" src="https://github.com/user-attachments/assets/bd29227e-512c-41fd-87bf-45d72893e49e" />


#####  5) после `kubectl apply -f ingress.yaml`
#####  6) Создание ALB минут 4-5 - это можно мониторить , а также `kubectl -n nginx-with-svc get ingress` - тоже мониторим
#####  7) далее из `kubectl -n nginx-with-svc get ingress`  и берем ip из ADDRESS  ---> его пишем в А запись ( в наш днс ) , в зависимости от TTL, наша страница должна стать доступнойпоявиться
Итог: <img width="1764" height="584" alt="image" src="https://github.com/user-attachments/assets/81288f21-0417-4d67-9a65-1eac8c16e5d0" />


