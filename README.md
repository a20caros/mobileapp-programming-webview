
Internet access:
På grund av att webview är en webbläsare i appen behöver appen ha tillåtelse av användaren att exempelvis få internetåtkomst. För att få tillåtelse av användaren implementerades det en extra behörighet i XML filen androidmanifest. I koden används uses-permissions för att kunna implementera extra behörigheter och genom android name anges det vad för extra behörighet som ska läggas till vilket i detta fallet var internet vilket anges i koden.

```
    <uses-permission android:name="android.permission.INTERNET" />
´´´

Skapad webview element i layoutfil:
Ett webview element implementerades in i layout XML filen content_main och ersattes av den redan ditlagda textview elementet. Inne i webview elementen angavs det ett id som fick namnet my_webview. Som layoutvy användes match_parent. Match_parent matchar både bredd och höjd samma som det överordnade attribut taggen.

```
     <WebView
         android:id="@+id/my_webview"
         android:layout_width="match_parent"
         android:layout_height="match_parent" />
```


Skapa privat medlemsvariabel och en ny klient:
Privata medlemsvariabler är variabler som enbart är visuell för den klass som den tillhör. I koden skapades det en privat medlemsvariabel med namnet mywebview med typen WebView. För att hitta id används metoden findviewById.

```
    private WebView mywebview;
```

```
    WebView mywebview = findViewById(R.id.my_webview);
    WebViewClient myWebViewClient = new WebViewClient();
    mywebview.setWebViewClient(myWebViewClient);
```

När en användare klickar på en länk ifrån webview är det ett standardbeteende i från Android att öppna ytterligare en app som tar hand om webbadresser. Men för att åstadkomma så att länkarna skall öppnas i webview används en webviewclient.

Aktivera Javascript-körning i webview klienten:
På grund av att Javascript är inaktiverat som standard i en webview behövdes detta aktiveras. Aktiveringen gjordes genom att använda websettings som bifogas till webview genom mywebview. Getsettings hämtar websettings som sedan aktiverar Javascript i webview genom setJavaScirptEnabled. Koden implementerades i mainactivity.

```
    WebSettings webSettings = mywebview.getSettings();
    webSettings.setJavaScriptEnabled(true);
```

Lägga till HTML-sida som en åtkomst:
För att en HTMl-fil skall kunna implementeras i appen behövdes en asset-folder skapas. Inuti asset-foldern skapades det en directory (katalogstruktur). I asset-foldern skapades det även en html fil som sedan kommer till att fyllas med html där en text vid exekvering kommer upp. Genom loadUrl anropas filen. Android_asset är mappen som html filen ligger i som heter mywebview.html.

```
    mywebview.loadUrl("file:///android_asset/mywebview.html");
```

Implementera ‘showExternalWebPage()’ och ‘showinternalWebPage()’:
I de redan implementerade metoderna showExternalWebPage och showinternalwebpage lades webview klienten till för att sidan som de olika webpages ska öppna skall öppnas i appen och inte i en webbläsare med en url. För metoden showExternalWebPage lades en loadUrl till med en extern URL. Den sidan som lades till var SVT. För showinternalwebpage lades html filen till för att öppnas när användaren klickar på showinternalwebpage i menyvalet.

```
    public void showExternalWebPage(){
       WebView mywebview = findViewById(R.id.my_webview);
       WebViewClient myWebViewClient = new WebViewClient();
       mywebview.setWebViewClient(myWebViewClient);
       mywebview.loadUrl("https://svt.se");
    }

    public void showInternalWebPage(){
       WebView mywebview = findViewById(R.id.my_webview);
       WebViewClient myWebViewClient = new WebViewClient();
       mywebview.setWebViewClient(myWebViewClient);
       mywebview.loadUrl("file:///android_asset/mywebview.html");
    }
```

För att sidorna ska komma upp när användaren väljer ett alternativ i menyn lades showExternalWebpage till i if-satsen action_external_web och showInternalWebPage lades till i if-satsen action_internal_web inuti Public boolean onOptionsItemSelected.

```
    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
       // Handle action bar item clicks here. The action bar will
       // automatically handle clicks on the Home/Up button, so long
       // as you specify a parent activity in AndroidManifest.xml.
       int id = item.getItemId();

       //noinspection SimplifiableIfStatement
       if (id == R.id.action_external_web) {
           Log.d("==>","Will display external web page");
           showExternalWebPage();
           return true;
       }

       if (id == R.id.action_internal_web) {
           Log.d("==>","Will display internal web page");
           showInternalWebPage();
           return true;
       }

       return super.onOptionsItemSelected(item);
    }
```

