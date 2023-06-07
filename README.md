# Async_Hillel


--------

## This Project Use Bank Api to Calculate Exchange Rate USD

--------

```python
import asyncio
import aiohttp


async def get_usd_1():
    print('Getting dollar exchange rate from Privat')
    async with aiohttp.ClientSession() as session:
        async with session.get('https://api.privatbank.ua/p24api/pubinfo?json&exchange&coursid=5') as response:
            data = await response.json()
            for currency in data:
                if currency['ccy'] == 'USD':
                    return float(currency['buy'])


async def get_usd_2():
    print('Getting dollar exchange rate from BankGov')
    async with aiohttp.ClientSession() as session:
        async with session.get('https://bank.gov.ua/NBUStatService/v1/statdirectory/exchange?json') as response:
            data = await response.json()
            for currency in data:
                if currency['cc'] == 'USD':
                    return float(currency['rate'])


async def get_usd_3():
    print('Getting dollar exchange rate from Monobank')
    async with aiohttp.ClientSession() as session:
        async with session.get('https://api.monobank.ua/bank/currency') as response:
            data = await response.json()
            for currency in data:
                if currency['currencyCodeA'] == 840:
                    return float(currency['rateBuy'])


async def get_exchange_rates():
    tasks = [
        get_usd_1(),
        get_usd_2(),
        get_usd_3()
    ]
    results = await asyncio.gather(*tasks)
    return results


async def main():
    exchange_rates = await get_exchange_rates()
    print(f"Exchange Rate USD: {exchange_rates}")
    average_rate = sum(exchange_rates) / len(exchange_rates)
    print(f"AVG Exchange Rate USD: {average_rate}")

asyncio.run(main())

```
# RESULT

```
Getting dollar exchange rate from Privat
Getting dollar exchange rate from BankGov
Getting dollar exchange rate from Monobank
Exchange Rate USD: [36.73, 36.5686, 36.65]
AVG Exchange Rate USD: 36.64953333333333
```