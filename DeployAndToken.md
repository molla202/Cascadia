![image](https://github.com/molla202/Cascadia/assets/91562185/ad3e3dfe-b3dd-46a2-a49f-49a2409d603b)



### Deploy Contract and mint token >>> SEND
NOT: işlemler sadece işleyişi anlamanız ve bilgi sahibi olmanız içindir. eksiklikler hatalar ve farklılıklar olabilir.
https://github.com/molla202/Cascadia/raw/main/cw20_base.wasm

* Öncelikle dosyamızı indiriyoruz
* Daha sonra deploy için siteye bağlanıyoruz >>> [BURAYA](https://cosmwasm.cascadia.foundation/contracts/upload/)
* Node kurduysanız o cüzdanı kullanabilirsiniz ağ üzerinde farklı cüzdan adresiniz çıkıyorsa öncelikle >>> [BURAYA](https://github.com/molla202/Cascadia/blob/main/farkl%C4%B1%20c%C3%BCzdan%20sorunu.md) bakınız
* Daha sonra dosyamızı yüklüyoruz ve upload diyoruz. cüzdandan onaylıyoruz. lürfen gas ayarı yapınız fail alıyorsanız keplr üzerinden  ayarlama kısmında 3 ile başlayanı 5 yapınız.
* Eğer hata verdiyse şuan sitede bir sorun var demektir. ancak işlemleri explorer üzerinden takip edin >>> [BURADAN](https://testnet.itrocket.net/cascadia)
* Contract ID nizi öğrenin ve site üzerinden resimlerde görülen işlemleri yapınız.
* Explorer üzerinden bilgileri alıyoruz. normalde site üzerinden bilgiler çıkıyordu ancak bir sorun var sanırım.
* Explorer üzerinde onaylanan işlem sırasında hash tıklayın ve en alta inin 3 nokta ... tıklayarak mesajları genişletip bilgilere ulaşabilirsiniz.



![image](https://github.com/molla202/Cascadia/assets/91562185/0dd74e57-99c7-4763-b2d8-91f3173815fe)
![image](https://github.com/molla202/Cascadia/assets/91562185/b24fba7f-5cde-41ca-970f-34793213ab9f)
![image](https://github.com/molla202/Cascadia/assets/91562185/b7c038d7-50ff-48ae-b5f4-d268b4754834)
![image](https://github.com/molla202/Cascadia/assets/91562185/7cec5d9a-0a9b-4b58-aee7-d54750e35c1c)
![image](https://github.com/molla202/Cascadia/assets/91562185/a01be971-6ceb-4384-8171-56eed8ee4cba)
![image](https://github.com/molla202/Cascadia/assets/91562185/d8ab4f0f-8e4c-4b4e-aa4d-5f26603c6e3a)
![image](https://github.com/molla202/Cascadia/assets/91562185/10ac67ec-dd97-465d-b96d-64dff85e45b3)
![image](https://github.com/molla202/Cascadia/assets/91562185/2991e3aa-e2af-4bc3-8e42-c5cb68ec47db)
![image](https://github.com/molla202/Cascadia/assets/91562185/e170f807-eec7-4062-b70b-a7ff7ef3445f)
![image](https://github.com/molla202/Cascadia/assets/91562185/ce76bc95-3a3f-4325-849e-929e92d55a1f)
![image](https://github.com/molla202/Cascadia/assets/91562185/1a91b339-12b4-400d-91fa-9b32a5c5e27d)



-------------------------------------------------------------------------------------------------------------
## Deploy and token mint >>> send 
### Kontratı sunucuya indirelim.
```
curl -sSL -o cw20_base.wasm https://github.com/molla202/Cascadia/raw/main/cw20_base.wasm
```
### Ağa yükleyelim
```
cascadiad tx wasm store ./cw20_base.wasm --from wallet --gas-adjustment 1.8 --chain-id cascadia_6102-1 --gas auto --gas-prices 150aCC -y
```
![image](https://github.com/molla202/Cascadia/assets/91562185/94b870d9-5941-47e9-a12c-7492ab8d8612)

### Tokenimizin ayarlamalarını yapalım ve akıllı sözleşmenin dağıtım işlemini yapalım
```
INIT='{"name":"isimyazın","symbol":"semboladıyazın","decimals":6,"initial_balances":[{"address":"ADRESİNİZİYAZIN","amount":"5000000"}],"mint":{"minter":"ADRESİNİZİYAZIN"},"marketing":{}}'
cascadiad tx wasm instantiate CODE-ID-NİZİ-YAZIN $INIT --from CUZDAN-ADINIZI-YAZIN --label "test" --gas-adjustment 1.5 --gas auto --gas-prices 70aCC --no-admin -y
```
![image](https://github.com/molla202/Cascadia/assets/91562185/2b16a87a-49c9-4dd3-a08c-43d1a7a8676f)
### Execute işlemi için komut varyasyonlarını yazalım
```
CONTRACT=$(cascadiad query wasm list-contract-by-code CODEIDNIZIYAZIN --output json | jq -r '.contracts[-1]')
```
```
TRANSFER='{"transfer":{"recipient":"GÖNDERMEKİSTEDİĞİNİZADRES","amount":"100"}}'
```
### Ve son olarak execute işlemi yapalım
```
cascadiad tx wasm execute $CONTRACT $TRANSFER --gas-adjustment 1.9 --gas-prices 150aCC --from cüzdan-adınız -y
```
NOT: ağda işlemlerimiz bu isimlerde görunuyor onaylandığındna emin olalım.
![image](https://github.com/molla202/Cascadia/assets/91562185/df0de9b4-afa3-4161-b7ec-3e7ba4a3e24f)
![image](https://github.com/molla202/Cascadia/assets/91562185/a8901644-f630-4990-8f6a-767220d0ca69)
![image](https://github.com/molla202/Cascadia/assets/91562185/8e07918c-9dc3-4dd5-bf40-2b5feb12c98c)













