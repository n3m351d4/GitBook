# Посты

1. **Золотые контакты**

![](../.gitbook/assets/image%20%286%29.png)

**2. Оборудование для оперативно-розыскной деятельности:**

* [http://www.bnti.ru/articles.asp?lvl=05.&p=1](http://www.bnti.ru/articles.asp?lvl=05.&p=1)
* [http://www.bnti.ru/showart.asp?aid=514&lvl=05](http://www.bnti.ru/showart.asp?aid=514&lvl=05)

**3.** [Лучшие из доступных станций с которыми я работала-это ](https://www.pace-shop.ru/pace-8007-0510?fee=28&fep=18236&gclid=Cj0KCQjw45_bBRD_ARIsAJ6wUXRIGa8jOOwfsXbjmxYECtdJpxcrUW3leURIMz8QKi8WwW_PsQ3N-p8aAunXEALw_wcB)**pace**

**4. Самые интересные наборы** [**Ардуино**](https://keyestudio.aliexpress.ru/store/1452162)\*\*\*\*

**5. Black Hat Tools** [**List** ](https://github.com/1522402210/2018-BlackHat-Tools-List)\*\*\*\*

**6. Дорк для Джитси**

![](../.gitbook/assets/image%20%281%29.png)

7.  П[ростенькая инъекция значений в пакетик счета игрушечки ](https://www.youtube.com/watch?v=o2vsjTyX2Qc)

```text
var url = window.location.pathname,
            username = gameeUI.user,
            timestamp = (new Date).getTime(),
            dataId = $("#dataId").data(),
            hash = CryptoJS.AES.encrypt(JSON.stringify({
                score: 7777788,
                timestamp: timestamp
            }), dataId.id, {
                format: CryptoJSAesJson
            }).toString(),
            sData = {
                score: 7777788,
                url: url,
                play_time: gameeUI.playTime,
                hash: hash,
                username: username,
                anonymous_id: gameeUI.anonymous_id
            };
        if (!isTelegram() && !isKik() && (isFacebook() || FacebookUserData && FacebookUserData.isLoggedIn() && !isViber())) {
            var fbUserData = FacebookUserData.getUserData();
            sData["facebook_user_id"] = fbUserData.app_scoped_user_id;
            sData["user_id"] = fbUserData.user_id
        }
        gameeUI.sendScoreData(sData)
```

8. **Слушать сдрки** [**http://websdr.org/**](http://websdr.org/)\*\*\*\*

 



