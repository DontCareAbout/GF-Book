
GWT module
==========

us.dontcareabout.gwt.GXT


設計動機
========

GXT `Chart` 的底層是 `DrawComponent`，搭配各式 `Sprite` 可以直接以繪圖的方式繪製畫面。
但是有幾個問題：

* 缺乏 layer 的概念：缺乏集體操作一組 `Sprite` 的能力。
* sprite event 難以處理：
	只有 `DrawComponent` 才能掛 handler、
	只能從 event 的 `getSprite()` 得知是哪個 `Sprite` 產生的 event，
	這會導致程式碼很難乾淨與改動。

基於這幾個問題，於是在既有的基礎上發展出一組 library 來解決這些問題，
並延伸出 component 以供使用。


基礎 class
==========

* `LSprite`：可供 `Layer` 操作的 sprite interface。
	原本的 `Sprite` 實作，如 `TextSprite` 會有對應的 `LSprite` 實作：`LTextSprite`。
	* 目前沒有提供 `PathSprite` 的對應實作... Orz
* `Layer`：`LSprite` 的集合，
	同一個 `Layer` 的 `LSprite` 會具有相同的新原點、zIndex 值、是否 hidden 等屬性。
* `LayerSprite`：同時具有 `Layer` 的 `LSprite` 實作。
	預設有一個 `LRectangleSprite` 作 background，因此有邏輯意義上的大小與可視範圍。
* `LayerContainer`：`DrawComponent` 的延伸替代品，GF-draw 的底層 container。


調整大小
========

GF-draw 的中心思想是：由 container 決定 element 的大小。


LayerContainer
--------------

`LayerContainer` 是 GXT `Component`，
因此決定其大小的行為與一般 GXT component 無異，在此不贅述。

child class 必須 override `onResize()`，
以便在 `LayerContainer` 改變大小時調整其 member。
這裡必須注意一點，在調整完之後必須呼叫 `super.onResize()`，
才能使 GXT 的 resize 機制完整執行。

```Java
	private LayerSprite member = new LayerSprite();
	
	@Override
	protected void onResize(int width, int height) {
		//讓 member 只有寬度的 80% 寬，並水平置中
		member.resize(width * 0.8, height);
		member.setLX(width * 0.1);
		
		//最後才能、也必須得呼叫 super.onResize()
		super.onResize(width, height);
	}
```


LayerSprite
-----------

override `adjustMember()` 可在該 instance 被呼叫 `resize()` 時調整 member。

```Java
	private List<LRectangleSprite> blocks = new ArrayList<LRectangleSprite>();
	
	@Override
	protected void adjustMember() {
		//水平排列，寬度均分
		double unitW = getWidth() / 1.0 / block.size();
		
		for (int i = 0; i < children.size(); i++) {
			LRectangleSprite rectangle = blocks.get(i);
			rectangle.setLX(unitW * i);
			rectangle.setLY(0);
			rectangle.resize(unitW, getHeight());
		}
	}
```


處理 event
==========

一樣可以處理 `SpriteSelectionEvent` 等 event，
只是 handler **不是** 掛在 `LayerContainer` 上，
而是由 `LayerSprite` 負責。

若同時有兩個 `LayerSprite` 都有掛 handler、邏輯上也都可以被觸發到，
例如 layer B 是 layer A 的 member，而觸發 event 的 sprite 在 layer B 上，
則視 layer A 的 `isStopPropagation()` 值來決定是否觸發 layer B 的 handler。

