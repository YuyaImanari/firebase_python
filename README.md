# Firebase を Python でいじるコードまとめ

## 0. 準備

### SDKを追加する

```bash
pip install firebase-admin
```

### SDKを初期化する

1. Firebaseのコンソールで `プロジェクトを設定` > `サービス アカウント` を開き、`新しい秘密鍵の生成` を選択

2. パスを通す。恒久的に使う場合は、`.zprofile` に書き込む。

   ```bash
   export GOOGLE_APPLICATION_CREDENTIALS="(パス名)"
   ```

3. 初期化

   ```python
   import firebase_admin
   from firebase_admin import credentials
   
   cred = credentials.ApplicationDefault()
   firebase_admin.initialize_app(cred, {
     'projectId': project_id,
   })
   ```


### 参考記事

- [サーバーに Firebase Admin SDK を追加する](https://firebase.google.com/docs/admin/setup?hl=ja#python)
- [Cloud Firestore を使ってみる  |  Firebase](https://firebase.google.com/docs/firestore/quickstart?hl=ja#python)

## 1.Firestoreデータの操作

### 準備

```python
from firebase_admin import firestore
db = firestore.client()
```

### 作成 or 上書き

```python
# ドキュメントIDを指定する場合
doc_ref = db.collection('collection_name').document('document_id')
doc_ref.set({
	'field_name': 'example_value',
  'timestamp': firestore.SERVER_TIMESTAMP
})

# ドキュメントIDを指定しない場合
db.collection('collection_name').add({
	'field_name': 'example_value',
  'timestamp': firestore.SERVER_TIMESTAMP
})
```

### 更新

```python
doc_ref = db.collection('collection_name').document('document_id')
doc_ref.update({
  'filed_name1': firestore.ArrayUnion(['example_value1'])
  'filed_name2': firestore.ArrayRemove(['example_value2'])
  'filed_name3': firestore.Increment(50)
})
```

### 参考記事

- [Cloud Firestore にデータを追加する  |  Firebase](https://firebase.google.cn/docs/firestore/manage-data/add-data?hl=ja#python_10)
- [Cloud Firestore でデータを取得する  |  Firebase](https://firebase.google.cn/docs/firestore/query-data/get-data?hl=ja#python)

## 2. Authenticationの管理

### 準備

```python
from firebase_admin import auth
```

### Authenticationの作成

```python
# すべての引数は任意
user = auth.create_user(
  uid = 'uid',
  email = 'user@example.com',
  password = 'password',
  display_name = 'Example Name',
)
```

### 参考記事

- [ユーザー管理  |  Firebase](https://firebase.google.com/docs/auth/admin/manage-users?hl=ja#python)
- [firebase_admin.auth module  |  Firebase](https://firebase.google.com/docs/reference/admin/python/firebase_admin.auth?hl=ja#create_user)