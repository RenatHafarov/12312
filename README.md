# Криптокошелёк в Telegram Web Application на базе TRON

## Оглавление

- [Введение](#введение)
- [Установка и Настройка](#установка-и-настройка)
- [Авторизация и Регистрация](#авторизация-и-регистрация)
- [Мониторинг баланса](#мониторинг-баланса)
- [Отправка и получение USDT и TRX](#отправка-и-получение-usdt-и-trx)
- [Bandwidth и Energy](#bandwidth-и-energy)
- [Заключение](#заключение)

## Введение

Данный проект представляет собой криптокошелёк на сети TRON, интегрированный с Telegram Web Application, используя библиотеку `tronweb`.

## Установка и Настройка

Для начала установите `tronweb`:

<pre>npm install tronweb</pre>

Импортируйте библиотеку:

<pre>const TronWeb = require('tronweb');</pre>

## Авторизация и Регистрация

### Регистрация

Для создания нового кошелька:

<pre>
const tronWeb = new TronWeb({
    fullHost: 'https://api.trongrid.io'
});

const newAccount = tronWeb.createAccount();
console.log(newAccount);
</pre>

После создания кошелька, шифруйте private key:

<pre>
const CryptoJS = require("crypto-js");
const encryptedKey = CryptoJS.AES.encrypt(newAccount.privateKey, 'your-secret-passphrase').toString();
</pre>

### Авторизация

Для авторизации используйте `address.base58`:

<pre>
const userAddress = "YOUR_USER_ADDRESS";
const isAddressValid = tronWeb.isAddress(userAddress);
</pre>

## Мониторинг баланса

Для проверки баланса:

<pre>
async function checkBalance(address) {
    const balanceTRX = await tronWeb.trx.getBalance(address);
    console.log(`Balance in TRX: ${tronWeb.fromSun(balanceTRX)}`);
}
</pre>

## Отправка и получение USDT и TRX

### Получение

Для получения монет, просто предоставьте ваш `address.base58`.

### Отправка

Для отправки монет:

<pre>
async function sendTRX(toAddress, amount) {
    const privateKey = "YOUR_PRIVATE_KEY";
    const transaction = await tronWeb.transactionBuilder.sendTrx(toAddress, amount);
    const signedTransaction = await tronWeb.trx.sign(transaction, privateKey);
    const result = await tronWeb.trx.sendRawTransaction(signedTransaction);
    console.log(result);
}
</pre>
## Bandwidth и Energy в TRON

**Bandwidth** и **Energy** являются двумя ключевыми ресурсами в сети TRON, которые используются для обработки и выполнения транзакций и контрактов.

### Bandwidth

- **Определение:** Bandwidth представляет собой ресурс, который используется для обработки транзакций в сети TRON. Каждая транзакция, которую вы отправляете в сеть, требует определенное количество Bandwidth.

- **Получение:** Когда вы замораживаете TRX, вы получаете Bandwidth в обмен. Замораживание TRX - это процесс, при котором вы временно блокируете свои монеты на определенный период времени в обмен на ресурсы сети.

- **Зачем это нужно:** Если у вас недостаточно Bandwidth, вы будете платить за транзакции в TRX. Если у вас достаточно Bandwidth (например, вы заморозили достаточное количество TRX), транзакции могут быть бесплатными.

### Energy

- **Определение:** Energy используется для выполнения и обработки смарт-контрактов в сети TRON. Это аналогично газу в сети Ethereum.

- **Получение:** Как и с Bandwidth, вы можете получить Energy, замораживая TRX. 

- **Зачем это нужно:** Выполнение смарт-контрактов требует вычислительной мощности. Energy представляет собой эту вычислительную мощность. Если у вас недостаточно Energy, выполнение смарт-контракта может стоить вам TRX.

**Примечание:** Важно помнить, что замораживание TRX дает вам либо Bandwidth, либо Energy, но не оба одновременно. Вы должны решить, какой ресурс вам нужнее, и заморозить TRX соответственно.

## Bandwidth и Energy

Для проверки Bandwidth и Energy:

<pre>
async function checkResources(address) {
    const accountResources = await tronWeb.trx.getAccountResources(address);
    console.log(`Bandwidth: ${accountResources.freeNetUsed}`);
    console.log(`Energy: ${accountResources.energyUsed}`);
}
</pre>

## Заключение

Это базовое руководство по созданию криптокошелька на сети TRON с интеграцией в Telegram Web Application. Для дополнительной информации и деталей обращайтесь к официальной документации `tronweb`.
