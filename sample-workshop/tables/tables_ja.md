# LiveLabsの表

## テーブルがカッコいい!

次のようにMarkdownで表を定義できます。

    | Tables        | Are           | Cool  |
    | ------------- |:-------------:| -----:|
    | **col 3 is**  | right-aligned | $1600 |
    | col 2 is      | *centered*    |   $12 |
    | zebra stripes | ~~are neat~~  |    $1 |
    

結果は次のようになります。

| テーブル | 次である | いいですね |
| --- | :-: | --: |
| **列3は** | 右揃え | $1600 |
| col 2は | _中央揃え_ | $12 |
| シマウマ | きちんとしている | $1 |

デフォルトの表キャプション(デフォルトではワークショップ・タイトルと演習タイトルの連結)が提供されていることがわかります。

デフォルトが気に入らない場合は、次の表定義を追加して、独自の表タイトルを指定することもできます。

    {: title="My table title"}
    

完全な値下げは次のようになります。

    | Tables        | Are           | Cool  |
    | ------------- |:-------------:| -----:|
    | **col 3 is**  | right-aligned | $1600 |
    | col 2 is      | *centered*    |   $12 |
    | zebra stripes | ~~are neat~~  |    $1 |
    {: title="My table title"}
    

テーブルは次のようになります。

| テーブル | 次である | いいですね |
| --- | :-: | --: |
| **列3は** | 右揃え | $1600 |
| col 2は | _中央揃え_ | $12 |
| シマウマ | きちんとしている | $1 |
| {: title="表タイトル"} |  |  |

このように、番号が自動的に追加されます。

これカッコいいじゃないですか。

[LiveLabs Markdown Cheatsheet](https://objectstorage.us-ashburn-1.oraclecloud.com/p/MKKRgodQ0WIIgL_R3QCgCRWCg30g22bXgxCdMk3YeKClB1238ZJXdau_Jsri0nzP/n/c4u04/b/qa-form/o/LiveLabs_MD_Cheat_Sheet.pdf)も参照してください。