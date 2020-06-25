# Изучение MQTT с использованием Wireshark

_Перевод: @N3M351DA_

_Источник:_ [_http://blog.catchpoint.com/201_](http://blog.catchpoint.com/201)\_\_

![http://blog.catchpoint.com/wp-content/uploads/2017/06/MQTT-Wireshark.jpg](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-1.jpeg)

Ранее мы рассмотрели протокол MQTT, как он работает и его роль в цифровом мире. Чтобы лучше понять содержание этой статьи, ознакомьтесь с основами межмашинного взаимодействия \(M2M\) в предыдущей статье «MQTT – Нервная Система IoT». Мы будем использовать монитор MQTT для отправления сообщений подписки/публикации от брокера MQTT. Изображение ниже иллюстрирует тест, настроенный с использованием [Catchpoint ](https://www.catchpoint.com/).

![http://blog.catchpoint.com/wp-content/uploads/2017/06/mqtt1.png](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-6.png)

Метрики мониторинга MQTT Catchpoint включают в себя время публикации, размер публикации, время подписки, размер подписки, DNS и время соединения.

![http://blog.catchpoint.com/wp-content/uploads/2017/06/mqtt2.png](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-7.png)

Чтобы интерпретировать данные, собранные с помощью мониторинга MQTT, вам нужно разобраться, что происходит при передаче сетевого траффика, это можно сделать с помощью Wireshark. Такие инструменты, как Wireshark, позволяют анализировать каждый шаг процесса и проверять пакеты данных. В этом анализе мы будем использовать монитор Catchpoint MQTT, чтобы настроить пробный тест.

**Анализ с использованием Wireshark**

Протокол MQTT основан на TCP / IP, и клиент и брокер должны поддерживать стек TCP / IP.

Ниже приведено изображение подписки с последующей публикацией в том же брокере MQTT. Мы будем рассматривать сообщение с уровнем доставки QoS t0. Давайте проанализируем сообщения, передаваемые с использованием этого протокола.

![http://blog.catchpoint.com/wp-content/uploads/2017/07/mqtt3.png](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-8.png)

Примечание. На изображении выше IP 192.168.0.11 – это IP-адрес брокера MQTT, а 192.168.0.12 – действует как клиент публикации и клиент подписки.

**1. Connect Message**

Соединение MQTT производится между клиентом и брокером и никогда напрямую с другим клиентом. Инициация этого соединения происходит с помощью команды CONNECT, отправляемой от клиента брокеру. Установленное соединение остается открытым до тех пор, пока не получит команду разъединения от клиента.

![http://blog.catchpoint.com/wp-content/uploads/2017/06/mqtt4.png](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-9.png)

Порт 1883 является портом по умолчанию для реализации протокола MQTT с помощью TCP.

Порт 8883 предназначен для реализации протокола MQTT с использованием TLS.

Рассмотрим детали сообщения подключения:

_**• Флаги заголовка:**_ содержат информацию о типе пакета управления MQTT.

_**• Флаги соединения:**_ байты флага соединения содержат параметры, определяющие поведение соединения MQTT. Он обозначает наличие или отсутствие полей в полезной нагрузке.

• _**Clean session:**_ первый бит флагов соединения. Этот флаг указывает посреднику, хочет ли клиент установить постоянное соединение или нет. Если установлено значение «истина», результатом является сеанс, при котором подписки удаляются при отключении, а при значении «ложь» может быть достигнуто надежное соединение, когда сообщения с высоким QoS доставляются при повторном подключении.

• _**Will flag:**_ второй бит флагов соединения. Часть механизма уведомления клиентов при потере соединения. При установке этот флаг означает, что, если запрос на соединение принят, на сервере должно быть сохранено сообщение Will. Сообщение Will – это сообщение MQTT с темой Will и сообщением Will. Оно используется при уведомлении других клиентов об отключении. Когда клиент отключается, брокер отправляет это сообщение от его имени. Когда флаг Will установлен в 1, серверы будут использовать поля Will QoS и Will Retain в флагах Connect.

• _**Will QoS:**_ биты 4 и 3 флагов подключения. Обозначают уровень QoS, который будет использоваться при публикации сообщения Will.

• _**Will retain:**_ Пятый бит флагов подключения. Если для параметра Will Retain установлено значение 0, сервер должен опубликовать сообщение Will без сохранения, а когда оно установлено в 1, сообщение Will публикуется как сообщение с сохранением.

• _**User Name and Password:**_ биты 7 и 6 флагов подключения. Когда эти поля установлены, сервер будет ожидать учетные данные в полезной нагрузке. MQTT позволяет отправлять имя пользователя и пароль для аутентификации клиента и авторизации. Пароль отправляется в виде открытого текста, если он не зашифрован.

• _**Keep alive:**_ Таймер поддержания активности используется для определения того, находится ли клиент MQTT в сети, для этого клиент отправляет брокеру регулярные сообщения запроса PING, а брокер отвечает PING-ответом.

• _**Client ID:**_ идентификатор клиента MQTT, подключающегося к брокеру MQTT. Он должен быть уникальным для каждого брокера.

• _**Payload:**_ полезная нагрузка содержит поля «Идентификатор клиента», «Тема», «Сообщение», «Имя пользователя» и «Пароль», наличие которых определяется флагами.

**2. Connect Acknowledgment Message**

Получив сообщение CONNECT, брокер отвечает сообщением CONNACK.

![http://blog.catchpoint.com/wp-content/uploads/2017/06/mqtt5.png](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-10.png)

• _**Header Flags**_**:** содержит информацию о типе пакета управления MQTT.

• _**Session Present:**_ бит 0 байта Connection Ack, является флагом присутствия сеанса. Этот флаг указывает, имеет ли брокер открытый сеанс клиента из предыдущих взаимодействий.

• _**Return Code**_: значения кода ответа и ответ в таблице ниже

![http://blog.catchpoint.com/wp-content/uploads/2017/06/mqtt6.png](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-11.png)

• _**Payload:**_ пакет CONNACK не имеет полезной нагрузки.

**3. Subscribe Message**

Клиент отправляет сообщение SUBSCRIBE брокеру MQTT для получения соответствующих ему сообщений.

![http://blog.catchpoint.com/wp-content/uploads/2017/06/mqtt7.png](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-12.png)

• _**Header Flags**_: содержат информацию о типе пакета управления MQTT.

• _**Message Identifier**_: идентификатор между клиентом и брокером, служит для идентификации сообщения в потоке сообщений. Актуально для QoS выше нуля.

• _**Topic and QoS Level:**_ Каждая подписка и тема представляет собой пару для фильтра тем и уровня QoS. Тема является предметом интереса клиента, который хотел бы получать сообщения о ней.

• _**Payload**_: полезная нагрузка содержит список подписок. Список подписок должен быть обязательно перечислен в пакете SUBSCRIBE.

**4. Subscribe Acknowledgement Message**

Брокер MQTT подтверждает подписку, отправляя подтверждение обратно клиенту в сообщении SUBACK.

![http://blog.catchpoint.com/wp-content/uploads/2017/06/mqtt8.png](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-13.png)

• _**Header Flags:**_ содержат информацию о типе пакета управления MQTT.

• _**Message Identifier:**_ актуально для сообщения с QoS больше нуля, аналогичным указанному в сообщении SUBSCRIBE.

• _**Return Code**_: MQTT-брокер отправляет код ответа для каждой пары Тема / QoS, полученной в сообщении SUBSCRIBE. Код ответа соответствует уровню QoS в случае успеха. Значения кода ответа приведены в таблице ниже.

![http://blog.catchpoint.com/wp-content/uploads/2017/06/mqtt9.png](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-14.png)

• _**Payload:**_ полезная нагрузка содержит список кодов ответа.

**5. Publish Message**

Как только клиент MQTT подключен к брокеру, он может публиковать сообщения.

![http://blog.catchpoint.com/wp-content/uploads/2017/06/mqtt10.png](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-15.png)

• _**Header Flags:**_ Содержат информацию о типе пакета управления MQTT.

• _**DUP flag:**_ Если флаг DUP равен 0, это означает, что это первая попытка отправки пакета PUBLISH. Если флаг равен 1, это указывает на повторную попытку отправки сообщения.

• _**QoS**_: Уровень QoS определяет уровень достоверности сообщения.

• _**Retain Flag:**_ Если данный флаг установлен в 1, сервер должен сохранить сообщение и его QoS, чтобы в дальнейшем обслуживать будущие подписки, соответствующие теме. Когда пакет PUBLISH отправляется подписывающемуся клиенту, сервер устанавливает Retain Flag в 1. Сервер устанавливает флаг сохранения в 0 при отправке пакета, если он соответствует установленной подписке, независимо от того, был ли установлен флаг при получении сообщения.

• _**Topic Name:**_ Строка UTF-8, которая также может содержать косую черту, если она иерархически структурирована. Публикуемое сообщение должно содержать тему, которая используется брокером для фильтрации по темам. Таким образом, брокер будет отправлять сообщения клиентам, которые подписались на указанную тему.

• _**Message**_: Полезная нагрузка вместе с темой, которая содержит фактические данные для передачи. Поскольку MQTT не зависит от передаваемых данных, полезная нагрузка может быть структурирована на основе варианта использования.

• _**Payload**_: Содержит публикуемое сообщение.

**6. Disconnect Request Message:**

Сообщение об отключении – это окончательный контрольный пакет, отправленный клиентом брокеру, оно означает отключение клиентом от брокера.

![http://blog.catchpoint.com/wp-content/uploads/2017/06/mqtt11.png](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-16.png)

• _**Header Flags:**_ содержат информацию о типе пакета управления MQTT.

• _**Payload:**_ пакет отключения не имеет полезной нагрузки.

**7. MQTT Keep Alive**

Функция поддержания активности \(Keep Alive\) гарантирует, что соединение открыто, а клиент и брокер связаны друг с другом. Когда соединение установлено, интервал поддержания активности \(в секундах\) передается брокеру клиентом.

В спецификации MQTT говорится:

_«Клиент отвечает за то, чтобы интервал между отправляемыми контрольными пакетами не превышал значения Keep Alive. При отсутствии отправки каких-либо других управляющих пакетов клиент должен отправить пакет PINGREQ»._

Поток Keep Alive поддерживается сообщениями PINGREQ и PINGRESP.

Типичный поток поддержания активности показан на рисунке ниже.

![http://blog.catchpoint.com/wp-content/uploads/2017/06/mqtt12.png](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-17.png)

**PINGREQ**

![http://blog.catchpoint.com/wp-content/uploads/2017/06/mqtt13.png](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-18.png)

PINGREQ посылается клиентом брокеру, чтобы показать, что он все еще активен, не смотря на то, что он не отправлял других пакетов управления MQTT. Если посредник не получает PINGREQ или какой-либо другой пакет, он закрывает соединение и отправляет сообщение LWT, если клиент его указал ранее.

• _**Header Flags**_: содержат информацию о типе пакета управления MQTT.

• _**Payload**_**:** пакет PINGREQ не имеет полезной нагрузки.

**PINGRESP**

PINGRESP – это ответ, который брокер отправляет в ответ на полученный пакет PINGREQ. Это указывает на доступность брокера для клиента.

![http://blog.catchpoint.com/wp-content/uploads/2017/06/mqtt14.png](http://dc7495.org/aybbtu/uploads/2020/04/http-blog-catchpoint-com-wp-content-uploads-2017-19.png)

**•** _**Header Flags**:_ содержат информацию о типе пакета управления MQTT.

**•** _**Payload**_: пакет PINGRESP не имеет полезной нагрузки.

**Заключение**

MQTT стимулирует инновации в сфере IoT и становится неотъемлемой частью цифрового мира. Предыдущий пост о MQTT дал нам обзор протокола, а в этой статье мы рассмотрели различные процессы, связанные с взаимодействием между клиентом MQTT и брокером MQTT с помощью Wireshark. Понимание этих метрик и рабочего процесса поможет вам быстро выявить проблемы, связанные с MQTT.
