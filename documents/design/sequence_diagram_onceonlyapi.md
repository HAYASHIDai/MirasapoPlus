@startuml
participant "クライアント（ブラウザ）"
box ミラサポ plus
participant "ミラサポplusサーバ(REST_API Endpoint)"
participant "ミラサポplusサーバ（認証）"
participant "ミラサポplusサーバ（DB（リソース））"
end box

box GビスID
participant "GビズID Auth Endpoint"
participant "GビズID Token Endpoint"
end box

activate "クライアント（ブラウザ）"
"クライアント（ブラウザ）" --> "ミラサポplusサーバ(REST_API Endpoint)" : Request（REST_API Endpoint）
activate "ミラサポplusサーバ(REST_API Endpoint)"
"ミラサポplusサーバ(REST_API Endpoint)" --> "ミラサポplusサーバ(REST_API Endpoint)" : Cookieチェック
"クライアント（ブラウザ）" <-- "ミラサポplusサーバ(REST_API Endpoint)" : RedirectURL（GビズID Auth Endpoint） /w state、nonce
deactivate "ミラサポplusサーバ(REST_API Endpoint)"
"クライアント（ブラウザ）" --> "GビズID Auth Endpoint" : Redirect
activate "GビズID Auth Endpoint"
"GビズID Auth Endpoint" --> "GビズID Auth Endpoint" : 認証チェック
"クライアント（ブラウザ）" <-- "GビズID Auth Endpoint" : 認証画面
"クライアント（ブラウザ）" --> "GビズID Auth Endpoint" : ID・PASS
"GビズID Auth Endpoint" --> "GビズID Auth Endpoint" : 認証チェック、code生成、Cookie発行
"クライアント（ブラウザ）" <-- "GビズID Auth Endpoint" : RedirectURL（ミラサポ plusコールバックURL） /w code、ログインセッションCookie
deactivate "GビズID Auth Endpoint"
"クライアント（ブラウザ）" --> "ミラサポplusサーバ（認証）" : Redirect /w code、state
activate "ミラサポplusサーバ（認証）"
"ミラサポplusサーバ（認証）" --> "ミラサポplusサーバ（認証）" : state検証
"ミラサポplusサーバ（認証）" --> "GビズID Token Endpoint" : Request access_token /w code
activate "GビズID Token Endpoint"
"GビズID Token Endpoint" --> "GビズID Token Endpoint" : code検証、token生成
"ミラサポplusサーバ（認証）" <-- "GビズID Token Endpoint" : access_token
deactivate "GビズID Token Endpoint"
"ミラサポplusサーバ（認証）" --> "ミラサポplusサーバ（認証）" : Cookie発行
"クライアント（ブラウザ）" <-- "ミラサポplusサーバ（認証）" : RedirectURL(REST_API Endopoint) /w cookie
deactivate "ミラサポplusサーバ（認証）"
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
