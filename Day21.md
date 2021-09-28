在使用k8s時，pod的數量與種類會隨著專案數量越來越多，管理的難度也會越來越高，這時就會需要一些分類的依據，而label就能勝任此工作

## 什麼是label
label是一個能加到object上的key/value組合，通常會用識別對使用者有意義以及相關的object，就像下面這樣：
```
"metadata": {
  "labels": {
    "key1" : "value1",
    "key2" : "value2"
  }
}
```
([來源](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/))

不過要注意，label通常用於有效的查詢和監控，理想上適用於UI以及CLI。而如果是沒有辨識性的資訊則建議使用[Annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/)

下面是一些常見的label 範例：
`"release" : "stable"`, `"release" : "canary"`
`"environment" : "dev"`, `"environment" : "qa"`, `"environment" : "production"`
`"tier" : "frontend"`, `"tier" : "backend"`, `"tier" : "cache"`
`"partition" : "customerA"`, `"partition" : "customerB"`
`"track" : "daily"`, `"track" : "weekly"`

如果想看更多範例可以去看[官網範例](https://kubernetes.io/docs/concepts/overview/working-with-objects/common-labels/),有更多常用的範例.

跟uid和names不同,label並沒有唯一性,因為通常情況之下會讓多個物件擁有相同的label,如果要透過label識別一組物件,就要透過Label selectors.

## 什麼是Label selectors
單看文字意思,自然是label所使用的選擇器(selectors),他能過濾label,目前api支援兩種組合:
equality-based 和 set-based 
1. equality-based:
    就是常見的判斷式,例如 `=`, `!=`, `==` 等等
2. set-based:
    範圍型的檢查,有 `in` `notin` 和 `exists` 
    
以下是範例:



透過這種方式,就可以撈出符合條件label的物件.


