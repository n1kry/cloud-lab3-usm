# Лабораторная работа #3 по теме "Облачные сети"

## Цель работы
Научиться вручную создавать виртуальную сеть (VPC) в AWS, добавлять в неё подсети, таблицы маршрутов, интернет-шлюз (IGW) и NAT Gateway, а также настраивать взаимодействие между веб-сервером в публичной подсети и сервером базы данных в приватной.

## Процесс выполнения работы
Прикрепляю скриншоты постепенного выполнения, по шагам.

## Шаг 1. Подготовка среды

<img width="1307" height="993" alt="1_1" src="https://github.com/user-attachments/assets/b27987fe-5219-48a2-821e-9e6b9d617d1a" />

## Шаг 2. Создание VPC

<img width="1307" height="940" alt="2_1" src="https://github.com/user-attachments/assets/03c466ec-7e5f-4ea7-ba68-3d1e2c6c1f19" />

- **Вопрос**: Что обозначает маска /16? - Маска `/16` означает, что первые 16 бит IP-адреса используются для адреса сети, а оставшиеся - для адресов хостов. Такая сеть позволяет создать большое количество подсетей и устройств внутри VPC.
- **Вопрос**: Почему нельзя использовать /8? - Использование `/8` создаёт слишком большой диапазон IP-адресов, что усложняет маршрутизацию и может привести к конфликтам с другими сетями AWS или локальными сетями.

## Шаг 3. Создание Internet Gateway (IGW)
Internet Gateway позволяет ресурсам внутри VPC выходить в Интернет. Без него публичные IP-адреса не будут работать. Если в виртуальной машине (EC2) назначен публичный IP, но нет IGW, она не сможет общаться с внешним миром.

<img width="1306" height="946" alt="3_1" src="https://github.com/user-attachments/assets/c99c2bbf-9548-434a-aa3c-41427f6c2e25" />

<img width="1306" height="424" alt="3_2" src="https://github.com/user-attachments/assets/a284b595-ef41-4b77-bf16-91d2f0674b78" />

<img width="1305" height="473" alt="3_3" src="https://github.com/user-attachments/assets/496a4b5c-22c4-44ad-a002-6bb269e562e8" />

## Шаг 4. Создание подсетей
Подсети (subnets) — это сегменты внутри VPC, которые позволяют изолировать ресурсы. То есть, подсети создаются для разделения ресурсов по функционалу и уровню доступа и для более гибкого управления трафиком.

## Шаг 4.1. Создание публичной подсети

<img width="1308" height="990" alt="4_1_1" src="https://github.com/user-attachments/assets/5426c953-5dee-4a53-a1fc-37c21d9703a4" />

<img width="1306" height="466" alt="4_1_2" src="https://github.com/user-attachments/assets/f4e5b350-34ed-4c4e-ab46-1de4238ca16e" />

- **Вопрос**: Является ли подсеть "публичной" на данный момент? Почему? - На данном этапе подсеть ещё не является публичной, так как для неё ещё не настроена таблица маршрутов с маршрутом через Internet Gateway.

## Шаг 4.2. Создание приватной подсети

<img width="1310" height="993" alt="4_2_1" src="https://github.com/user-attachments/assets/4e465040-8852-4c37-81f3-fe9e8502006f" />

<img width="1305" height="487" alt="4_2_2" src="https://github.com/user-attachments/assets/53715a3d-0e95-40b7-bf6a-18e5e66a78f1" />

- **Вопрос**: Является ли подсеть "приватной" на данный момент? Почему? - На данном этапе подсеть фактически является приватной, так как не имеет маршрута в Интернет.

## Шаг 5. Создание таблиц маршрутов
По умолчанию каждая новая VPC имеет одну основную таблицу маршрутов, и все новые подсети автоматически к ней подключаются.

## Шаг 5.1. Создание публичной таблицы маршрутов

<img width="1308" height="570" alt="5_1_1" src="https://github.com/user-attachments/assets/0bc1dbff-035c-46b6-99b6-5a61c50802f4" />

<img width="1308" height="568" alt="5_1_2" src="https://github.com/user-attachments/assets/291c2edf-c59e-47f5-afe0-9767986a5a38" />

<img width="1309" height="434" alt="5_1_3" src="https://github.com/user-attachments/assets/8ccafa2d-92d7-4444-ac26-f50ec18dab41" />

<img width="1305" height="475" alt="5_1_4" src="https://github.com/user-attachments/assets/d4838f89-c3d3-43e3-a5d7-b3758b98a6ee" />

<img width="1304" height="562" alt="5_1_5" src="https://github.com/user-attachments/assets/cd42bb15-bc58-46d1-8661-72a5935f51db" />

<img width="1305" height="657" alt="5_1_6" src="https://github.com/user-attachments/assets/67efc223-7ffd-471a-b68a-b9bd67ce39d2" />

- **Вопрос**: Зачем необходимо привязать таблицу маршрутов к подсети? - Таблица маршрутов определяет направление сетевого трафика для конкретной подсети. Без привязки подсеть не будет использовать заданные маршруты.

## Шаг 5.2. Создание приватной таблицы маршрутов

<img width="1303" height="573" alt="5_2_1" src="https://github.com/user-attachments/assets/53dfd610-0add-4684-8ac4-78ad5f859acd" />

<img width="1305" height="471" alt="5_2_2" src="https://github.com/user-attachments/assets/51aac659-9eb6-4229-90a7-d3b670525bcc" />

<img width="1306" height="703" alt="5_2_3" src="https://github.com/user-attachments/assets/f819e8f4-f9b0-49e1-b7e4-0fc8dc66f458" />

## Шаг 6. Создание NAT Gateway
NAT Gateway позволяет ресурсам в приватной подсети выходить в Интернет (например, для обновления ПО), при этом оставаясь недоступными извне.

- **Вопрос**: Как работает NAT Gateway? - NAT Gateway позволяет инстансам в приватной подсети выходить в Интернет для обновлений и загрузки пакетов, оставаясь недоступными извне.

## Шаг 6.1. Создание Elastic IP
Elastic IP — это статический публичный IPv4-адрес, закреплённый за вашим аккаунтом AWS. Он используется для NAT Gateway, чтобы тот мог представлять собой “точку выхода” в Интернет от имени всех приватных инстансов.

<img width="1308" height="506" alt="6_1_1" src="https://github.com/user-attachments/assets/612d7f43-6287-4c1b-b810-4408fe80f633" />

<img width="1305" height="447" alt="6_1_2" src="https://github.com/user-attachments/assets/867f3e78-32f8-4b49-b1a4-9942d6afcfc7" />

## Шаг 6.2. Создание NAT Gateway

<img width="1308" height="502" alt="6_2_1" src="https://github.com/user-attachments/assets/88f0d307-5191-4b44-ba18-eab2cc9c1282" />

<img width="1307" height="871" alt="6_2_2" src="https://github.com/user-attachments/assets/f541e413-c671-41f6-a7d3-e84c234b0ed8" />

<img width="1306" height="333" alt="6_2_3" src="https://github.com/user-attachments/assets/19b432b6-b08e-45e4-bc40-d41d107ea6c8" />

## Шаг 6.3. Изменение приватной таблицы маршрутов

<img width="1306" height="629" alt="6_3_1" src="https://github.com/user-attachments/assets/7534ff97-39bb-4670-957a-2a47042861a0" />

<img width="1305" height="424" alt="6_3_2" src="https://github.com/user-attachments/assets/cea8b398-1b34-4cf4-9702-345a9d4222ae" />

<img width="1306" height="561" alt="6_3_3" src="https://github.com/user-attachments/assets/561c4738-4617-42d3-8201-9aeb7e67a385" />

## Шаг 7. Создание Security Groups
Security Group - это виртуальный брандмауэр на уровне инстанса (EC2), который контролирует входящий (Inbound) и исходящий (Outbound) трафик.

<img width="1302" height="616" alt="7_1" src="https://github.com/user-attachments/assets/a3129cae-51fd-41c8-b8fb-3bf004c6c744" />

<img width="1304" height="963" alt="7_2" src="https://github.com/user-attachments/assets/f99b976d-65b2-46f0-b688-38965e081eca" />

<img width="1307" height="962" alt="7_3" src="https://github.com/user-attachments/assets/ebae629b-8790-42ca-bb63-6ab45a6c5932" />

<img width="1305" height="965" alt="7_4" src="https://github.com/user-attachments/assets/e95b83f1-9553-42a6-a466-0040ba929534" />

<img width="1304" height="547" alt="7_5" src="https://github.com/user-attachments/assets/258ab2a4-211b-4616-86e6-e7687a0d8248" />

- **Вопрос**: Что такое Bastion Host и зачем он нужен в архитектуре с приватными подсетями? - Bastion Host - это промежуточный сервер в публичной подсети, через который осуществляется безопасный доступ к ресурсам в приватной подсети.

## Шаг 8. Создание EC2-инстансов

<img width="1310" height="428" alt="8_1" src="https://github.com/user-attachments/assets/9dd38993-7d01-4364-8d90-d7466160e2b6" />

<img width="1304" height="966" alt="8_2" src="https://github.com/user-attachments/assets/d06e981a-d8b8-4372-a5c5-c7e7387aabe0" />

<img width="1300" height="956" alt="8_3" src="https://github.com/user-attachments/assets/827f85bd-cea8-4110-9901-8d05849814e0" />

<img width="1307" height="989" alt="8_4" src="https://github.com/user-attachments/assets/b2e300f1-5244-4441-812b-812469aa3144" />

<img width="1308" height="992" alt="8_5" src="https://github.com/user-attachments/assets/19529e1f-1098-47b4-bca6-9c7d77b41dab" />

<img width="1306" height="963" alt="8_6" src="https://github.com/user-attachments/assets/e939e208-ddb5-4b33-81fa-c850d58a45e8" />

<img width="1306" height="963" alt="8_7" src="https://github.com/user-attachments/assets/5dd1a625-7cba-4192-9e65-748d9b07c707" />

<img width="1307" height="963" alt="8_8" src="https://github.com/user-attachments/assets/604e1770-eab9-479f-9f10-762f11b40d53" />

<img width="1305" height="963" alt="8_9" src="https://github.com/user-attachments/assets/8065b709-2262-41cb-93e3-5b2b1a69f070" />

<img width="1305" height="960" alt="8_10" src="https://github.com/user-attachments/assets/147d08d4-771e-43ae-bc4f-00317c69d42b" />

<img width="1307" height="506" alt="8_11" src="https://github.com/user-attachments/assets/c912ab29-bfc0-4134-9f25-619a140e46c1" />

## Шаг 9. Проверка работы

<img width="1290" height="987" alt="9_1" src="https://github.com/user-attachments/assets/4bc6387a-ff18-4fd2-a39e-cfabe1951c52" />

<img width="995" height="429" alt="9_2" src="https://github.com/user-attachments/assets/a40eb027-905d-4f89-98f3-851ef37fd1b7" />

<img width="735" height="182" alt="9_3" src="https://github.com/user-attachments/assets/757a1e7e-2a0e-4aba-bd15-19fd3c48b7bd" />

# Вывод

В ходе лабораторной работы были изучены основные принципы построения сетевой инфраструктуры в AWS. Была вручную создана виртуальная сеть VPC с публичной и приватной подсетями, настроены Internet Gateway, NAT Gateway и таблицы маршрутов.

Также были получены практические навыки настройки Security Groups, создания EC2-инстансов и организации безопасного взаимодействия между публичными и приватными ресурсами через Bastion Host.

В результате была успешно реализована облачная архитектура с разделением сетевого доступа и безопасным подключением к внутренним ресурсам AWS.
