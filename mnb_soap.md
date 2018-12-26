# Inspect SOAP of MNB interface

## Install Prerequirements
```
$ virtualenv mnb 
$ source mnb/bin/activate
$ curl https://bootstrap.pypa.io/get-pip.py | python
$ pip install zeep
```

## Retrieve info about the published web service
```
$ python -mzeep http://www.mnb.hu/arfolyamok.asmx?wsdl

Bindings:
     Soap11Binding: {http://tempuri.org/}CustomBinding_MNBArfolyamServiceSoap

Service: MNBArfolyamServiceSoapImpl
     Port: CustomBinding_MNBArfolyamServiceSoap (Soap11Binding: {http://tempuri.org/}CustomBinding_MNBArfolyamServiceSoap)
         Operations:
            GetCurrencies() -> GetCurrenciesResult: xsd:string
            GetCurrencyUnits(currencyNames: xsd:string) -> GetCurrencyUnitsResult: xsd:string
            GetCurrentExchangeRates() -> GetCurrentExchangeRatesResult: xsd:string
            GetDateInterval() -> GetDateIntervalResult: xsd:string
            GetExchangeRates(startDate: xsd:string, endDate: xsd:string, currencyNames: xsd:string) -> GetExchangeRatesResult: xsd:string
            GetInfo() -> GetInfoResult: xsd:string
```

## Retrieve info via the methods generated from wsdl:
```
>>> from zeep import Client

>>> client = Client('http://www.mnb.hu/arfolyamok.asmx?wsdl' );

>>> client.service.GetDateInterval()
'<MNBStoredInterval><DateInterval startdate="1949-01-03" enddate="2018-12-21" /></MNBStoredInterval>'

>>> client.service.GetExchangeRates( "2018-12-13", "2018-12-15","USD");
'<MNBExchangeRates><Day date="2018-12-14"><Rate unit="1" curr="USD">286,66</Rate></Day><Day date="2018-12-13"><Rate unit="1" curr="USD">284,34</Rate></Day></MNBExchangeRates>'
```

Note: If no price info is available, take previous known (see https://www.mnb.hu/statisztika/statisztikai-adatok-informaciok/adatok-idosorok/arfolyamok-lekerdezese/a-hivatalos-devizaarfolyamok-megallapitasa-es-publikalasa)
