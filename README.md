
# Betaal Systeem - Project door Justin

Dit is de documentatie van het project met de ABN Amro API. (ING Lukte niet helemaal...)

## Installation

Voor TEST requests, please gebruik Postman. In deze topic leg ik uit wat je nodig hebt voor een Default request.

Eerst moet je de Certificaten downloaden. Die kan je hier downloaden (beide moet je hebben)

[CRT Certificate](https://developer.abnamro.com/sites/default/files/2020-05/PSD2TPPCertificate.crt) 

[KEY Certificate](https://developer.abnamro.com/sites/default/files/2020-05/PSD2TPPprivateKey.key)

## Usage

Als je je API Key hebt kan je deze invullen voor dit basic request gebruik ik mijn API-Key (test).

Om een test rekening te berijken klik [hier](https://auth-sandbox.connect.abnamro.com/as/authorization.oauth2?scope=psd2:account:balance:read+psd2:account:transaction:read+psd2:account:details:read&client_id=TPP_test&response_type=code&flow=code&redirect_uri=https://localhost/auth&bank=NLAA01&state=SilverAdministration-123
) en je word gerederict naar een pagina, daar kies je een rekening (pak de eerste `NL12ABNA9999876523`). Nadat je dit doet word je gerederict naar een LocalHost URL. Hierin staat: 
```bash
https://localhost/auth?code=9C6UrsGZ0Z3XJymRAOAgl7hKPLlWKUo9GBfMQQEs&state=SilverAdministration-123
```
In deze URL staat een "code". Deze code is 60 seconden geldig, dus wees snel 😜. in het voorbeeld is de code: `9C6UrsGZ0Z3XJymRAOAgl7hKPLlWKUo9GBfMQQEs`. Deze code gaan we later nodig hebben. 

als je deze code hebt kan je deze in postman een request maken om dit om te zetten naar een Access Token, Deze is 2 uur geldig. 

Nadat je deze token hebt kan je requests maken! :D. \
Bij de Access Token krijg je ook Refresh Token, deze heb je ook nodig. \
Hier is een voorbeeld hoe je de refresh token gebruikt:
```
curl -X POST -k https://auth-sandbox.connect.abnamro.com:8443/as/token.oauth2 \
-v \
--cert TPPCertificate.crt \
--key TPPprivateKey.key \
-H 'Cache-Control: no-cache' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=refresh_token&client_id=TPP_test&refresh_token=[REFRESH_TOKEN_HERE]&scope=psd2:account:balance:read&psd2:account:transaction:read'
```
Vervang de `[REFRESH_TOKEN_HERE]` Met de RefreshToken. \
Voor requests (account balance ect) gebruik je GET Requests,
Hier zijn een aantal requests:

Get recent transactions:
```
curl -X GET "https://api-sandbox.abnamro.com/v1/accounts/NL12ABNA9999876523/transactions?bookDateFrom=2019-02-22&bookDateTo=2019-02-28" \
-H 'Authorization: Bearer [AUTH_TOKEN_HERE]' \
-H 'API-Key: [API_KEY_HERE]'
```

Get current balance:
```
curl -X GET -k https://api-sandbox.abnamro.com/v1/accounts/NL12ABNA9999876523/balances \
-H 'Authorization: Bearer [AUTH_TOKEN_HERE]' \
-H 'API-Key: [API_KEY_HERE]'
```

Get account information:
```
curl -X GET -k https://api-sandbox.abnamro.com/v1/accounts/NL12ABNA9999876523/details \
-H 'Authorization: Bearer [AUTH_TOKEN_HERE]' \
-H 'API-Key: [API_KEY_HERE]'
```

Vervang de `[AUTH_TOKEN_HERE]` met de authtoken (eerder verkregen) en vervang de `[API_KEY_HERE]` met jou api key van de [ABN Amro Developer Site](https://developer.abnamro.com). Je kan eventueel bij de `Get recent transactions` in de request url de date aanpassen.

Voor Postman requests, ``-H`` behoort tot de HEADER, ``-d`` behoort tot de BODY

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)
