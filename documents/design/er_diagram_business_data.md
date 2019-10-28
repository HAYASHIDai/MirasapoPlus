```uml
@startuml
!define MAIN_ENTITY            #E2EFDA-C6E0B4
!define MAIN_ENTITY_2          #FCE4D6-F8CBAD
!define METAL                  #F2F2F2-D9D9D9
!define MASTER_MARK_COLOR      AAFFAA
!define TRANSACTION_MARK_COLOR FFAA00

skinparam class {
    BackgroundColor METAL
    BorderColor     Black
    ArrowColor      Black
}


entity "会員情報" as member <<M,MASTER_MARK_COLOR>> MAIN_ENTITY {
    + ユーザーID [PK]
    --
    ユーザー氏名(姓)
    ユーザー氏名ヨミガナ(姓)
    ユーザー氏名(名)
    ユーザー氏名ヨミガナ(名)
    メールアドレス
    所属
    ユーザー区分
    活動地域
    興味のある項目
    興味のある項目(その他)
    作成日時
    更新日時
    更新情報源
}
note left:ユーザー区分は「中小企業・小規模事業者」か「その他」

entity "事業基本情報" as business_basic <<M,MASTER_MARK_COLOR>> {
    + 事業ID [PK]
    --
    # ユーザーID [FK]
    商号又は名称
    商号又は名称(カナ）
    本社所在地
    本社建物名
    業種コード
    代表者役割
    代表者名(姓)
    代表者名(名)
    設立日
    担当者部署
    担当者役職
    担当者名(姓)
    担当者名ヨミガナ(姓)
    担当者名(名)
    担当者名ヨミガナ(名)
    電話番号
    内線
    メールアドレス
    連絡先住所
    webフォームURLアドレス
    作成日時
    更新日時
    更新情報源
}

entity "事業財務情報" as business_finance <<M,MASTER_MARK_COLOR>> {
    + 事業財務情報ID [PK]
    --
    # 事業ID [FK]
    決算年月日
    事業期間開始日
    事業期間終了日
    期末従業員数
    期末正社員数
    資産合計
    流動資産合計
    現金及び預金
    受取手形
    売掛金
    有価証券
    棚卸資産
    その他流動資産合計
    前払金
    短期貸付金
    貸倒引当金
    固定資産合計
    有形固定資産合計
    土地
    無形固定資産合計
    その他固定資産合計
    貸付金合計
    負債合計
    流動負債合計
    支払手形
    買掛金
    短期借入金
    その他流動負債合計
    未払金
    前受金
    預り金
    固定負債合計
    長期借入金
    純資産合計
    資本金
    資本準備金
    売上高
    売上原価
    売上原価内減価償却費
    労務費
    売上総利益
    販売費及び一般管理費 
    販管費内減価償却費
    人件費
    営業利益
    営業外収益
    営業外費用
    経常利益
    特別利益
    特別損失
    税引前当期純利益
    当期純利益
    作成日時
    更新日時
    更新情報源
}


entity "事業株主" as shareholder <<M,MASTER_MARK_COLOR>> {
    + 事業株主ID [PK]
    --
    # 事業ID
    法人名もしくは出資者(姓)
    法人名もしくは出資者ヨミガナ(姓)
    法人名もしくは出資者(名)
    法人名もしくは出資者ヨミガナ(名)
    法人番号
    住所
    出資比率
    大企業区分
    作成日時
    更新日時
    更新情報源
}

entity "事業役員" as officer <<M,MASTER_MARK_COLOR>> {
    + 役員ID [PK]
    --
    # 事業ID
    役職
    役員名(姓)
    役員名ヨミガナ(姓)
    役員名(名)
    役員名ヨミガナ(名)
    生年月日
    性別
    大企業区分
    作成日時
    更新日時
    更新情報源
}

entity "事業所" as office <<M,MASTER_MARK_COLOR>> {
    + 事業所ID [PK]
    --
    # 事業ID
    事業所名
    事業所所在地
    事業所建物名
    事業所郵便番号
    作成日時
    更新日時
    更新情報源
}

member            ||-ri-o|     business_basic
business_basic    ||-ri-o{     business_finance
business_basic    ||-do-o{     shareholder
business_basic    ||-do-o{     officer
business_basic    ||-do-o{     office
@enduml
```
