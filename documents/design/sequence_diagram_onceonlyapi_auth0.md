```uml
@startuml
participant "クライアント（ブラウザ）"
participant "Auth0"
box ミラサポ plus
participant "ミラサポplusサーバ(REST_API Endpoint)"
participant "ミラサポplusサーバ（DB（リソース））"
end box

box GビスID
participant "GビズID Auth Endpoint"
participant "GビズID Token Endpoint"
end box

activate "クライアント（ブラウザ）"
"クライアント（ブラウザ）" --> "Auth0" : Request
activate "Auth0"
"Auth0" --> "Auth0" : stateチェック
"クライアント（ブラウザ）" <-- "Auth0" :  ユニバーサルログイン画面
deactivate "Auth0"
"クライアント（ブラウザ）" --> "GビズID Auth Endpoint" : リンククリック
activate "GビズID Auth Endpoint"
"GビズID Auth Endpoint" --> "GビズID Auth Endpoint" : 認証チェック
"クライアント（ブラウザ）" <-- "GビズID Auth Endpoint" : 認証画面
"クライアント（ブラウザ）" --> "GビズID Auth Endpoint" : ID・PASS
"GビズID Auth Endpoint" --> "GビズID Auth Endpoint" : 認証チェック、code生成、Cookie発行
"クライアント（ブラウザ）" <-- "GビズID Auth Endpoint" : RedirectURL（Auth0コールバックURL） /w code、ログインセッションCookie
deactivate "GビズID Auth Endpoint"
"クライアント（ブラウザ）" --> "Auth0" : Redirect /w code、state
activate "Auth0"
"Auth0" --> "Auth0" : state検証
"Auth0" --> "GビズID Token Endpoint" : Request access_token /w code
activate "GビズID Token Endpoint"
"GビズID Token Endpoint" --> "GビズID Token Endpoint" : code検証、token生成
"Auth0" <-- "GビズID Token Endpoint" : access_token
deactivate "GビズID Token Endpoint"
"Auth0" --> "Auth0" : Cookie発行
"クライアント（ブラウザ）" <-- "Auth0" : RedirectURL(REST_API Endopoint) /w cookie
deactivate "Auth0"
"クライアント（ブラウザ）" --> "ミラサポplusサーバ(REST_API Endpoint)" : Request（REST_API Endpoint） /w cookie
activate "ミラサポplusサーバ(REST_API Endpoint)"
"ミラサポplusサーバ(REST_API Endpoint)"  --> "ミラサポplusサーバ(REST_API Endpoint)" : Cookieチェック
"ミラサポplusサーバ(REST_API Endpoint)" --> "ミラサポplusサーバ（DB（リソース））" : DBリクエスト
activate "ミラサポplusサーバ（DB（リソース））" 
"ミラサポplusサーバ(REST_API Endpoint)" <-- "ミラサポplusサーバ（DB（リソース））" : DBレスポンス
deactivate "ミラサポplusサーバ（DB（リソース））" 

"クライアント（ブラウザ）" <-- "ミラサポplusサーバ(REST_API Endpoint)" : Response(JSON)
deactivate "ミラサポplusサーバ(REST_API Endpoint)"
@enduml
```
