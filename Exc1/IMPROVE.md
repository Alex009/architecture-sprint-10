# Что можно улучшить

> Составьте список данных для защиты и проставьте для каждого способы защиты (шифрование, обфускация, обезличивание). Разработайте механизм тегирования данных с использованием инструментов тегирования. Составьте список инструментов, способов и мер, которые позволят обеспечить конфиденциальность данных в указанных потоках. Доработайте диаграммы из предыдущего шага — отобразите на них, что следует использовать на каждом этапе потока.

## Данные для защиты
- Персональные данные - ФИО, телефон, email, дата рождения
    - при хранении и передаче - шифрование
    - при передаче для бизнес-анализа - обезличивание (даты рождения оставляем, а ФИО, телефон и емейл переводим в маску *****)
    - ограничение доступа к данным по ролям
- Паспортные данные, адрес регистрации, прописка, работа
    - при хранении и передаче - шифрование
    - ограничение доступа к данным по ролям (только для ресепшена, так как они занимаются договорами с клиентами)
    - аудит доступа к данным
- Полис ОМС
    - при хранении и передаче - шифрование
    - при передаче для бизнес-анализа - маскирование информации кроме страховщика и срока действия полиса
    - ограничение доступа к данным по ролям (только для ресепшена, так как они занимаются договорами с клиентами)
    - аудит доступа к данным
- Медицинская информация (медкарта, анализы, заключения)
    - при хранении и передаче - шифрование
    - ограничение доступа к данным по ролям (только для врачей, так как они занимаются лечением)
    - аудит доступа к данным
    - при передаче для бизнес-анализа - обезличивание (даты рождения обрезаем до года, информацию о диагнозе/анализах сводим до минимально необходимого для анализа - например категория болезни)
- Договора и согласия клиентов
    - при хранении и передаче - шифрование
    - ограничение доступа к данным по ролям (только для ресепшена, так как они занимаются договорами с клиентами)
    - аудит доступа к данным
- Финансовые данные пациентов
    - шифрование базы данных 1С (например дисковое шифрование файла)
    - аудит и ограничения прав есть в самой 1C
- Конфиденциальные данные сотрудников и компании
    - шифрование базы данных 1С (например дисковое шифрование файла)
    - аудит и ограничения прав есть в самой 1C

## Тегирование данных

Модель тегирования:
- Public - безопасная для публичного доступа информация (инструкции или рекламные материалы, которые всё равно печатаются и вешаются в офисе для всех)
- Internal - внутренние данные компании, но не являющиеся конфиденциальными
- Confidential L1 - конфиденциальные данные сотрудников. Понесем ущерб при утечке, но понятного ограниченного масштаба.
- Confidential L2 - конфиденциальные данные пациентов. Понесем значительный ущерб при утечке.
- Confidential L3 - наиболее критичные конфиденциальные данные. Их утечка приведет к критическим последствиям. К ним относятся медицинские данные пациентов, коммерческая тайна компании, банковские данные пациентов.

Инструменты для применения тегирования - [Microsoft Purview](https://learn.microsoft.com/ru-ru/purview/information-protection), так как вся инфраструктура сейчас базируется на использовании Windows Server и Excel. С его помощью решается и задача тегирования и задача шифрования конфиденциальных данных.

## Аудит

В Windows Server есть возможность включить аудит доступа к файлам на сетевом диске, что позволит видеть кто и когда работал с файлами.

## Ограничение доступа

1. Настроить группы безопасности в Active Directory для учетных записей сотрудников - разбить их по группам согласно ролям в организации (врач, ресепшен, бухгалтерия и т.д.)
2. На общем диске настроить разрешения для общих папок таким образом, чтобы выполнялось правило минимальных разрешений. Например медицинская карта доступна для чтения и изменения только врачам, в то время как ресепшен с ней не работает. Windows server поддерживает много возможностей по ограничению. Например можно сделать чтобы ресепшен видел список файлов пациента, но не мог их читать, однако мог добавлять новые.

Описанные выше пункты снизят риски на то время, пока будет готовиться полноценная замена текущему решению, с более строгим ограничением доступа к данным. 
Пункты выше не решают проблему файлов учета например. Ресепшен будет видеть всё равно полное содержимое файла учета пациентов, без возможности понять а что именно конкретный сотрудник ресепшена прочитал/изменил в учете.

## Ограничение доступа в серверное помещение

Серверное помещение находится в том же здании. Нужно ограничить доступ, поставить систему контроля доступа и журнал входа/выхода.
