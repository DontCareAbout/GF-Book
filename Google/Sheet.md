

GWT module
==========

us.dontcareabout.gwt.GWT


使用方式
========

假設試算表的 **第 N 個** 工作表的欄位名稱（第一列各 cell 的值）如下：

* 日期
* 網址
* score
* 權重


定義一個對應 VO，需繼承 `SheetEntry`，例如：

```Java
public final class Foo extends SheetEntry {
	protected Foo() {}	//JSNI: JavaScriptObject 規格要求
	
	public Date getDate() {
		return dateField("日期");
	}
	
	public String getUrl() {
		return stringField("網址");
	}
	
	public Integer getScore() {
		return intField("score");
	}
	
	public Double getWeight() {
		return doubleField("weight");
	}
}
```


`SheetEntry` 是一個 `JavaScriptObject`，所以必須遵守 JSNI 規範。
細節可參閱[此份文件][Wiki-GWT]的「JSON 相關」段落。
為了避免程式運行時的 casting 困擾，
因此 `SheetEntry` 提供 `dateField()` 等 method 協助讀取欄位並轉換至對應型態。


定義好 VO，則可透過 `SheetHappen` 讀取資料：

```Java
SheetHappen.<Foo>get(sheetId, N, new Callback<Foo>() {
	@Override
	public void onSuccess(Sheet<Foo> gs) {
		ArrayList<Foo> data = gs.getEntry();
	}

	@Override
	public void onError(Throwable exception) {}
});
```


[Wiki-GWT]: https://gwt.dontcareabout.us/GWT/