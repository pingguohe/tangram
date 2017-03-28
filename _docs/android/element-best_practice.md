---
title: "Best Practice - 组件构建"
permalink: /docs/android/element-best_practice
excerpt: "Best Practice - 组件构建"
modified: 2016-11-03T10:01:43-04:00
redirect_from:
  - /theme-setup/
---

## 组件样例
显示一个带组件位置的文本组件

### 数据结构

```

{
    "type":"1",
    //内部的Url
    "contentUrl":"https://www.google.com.hk",
    //跳转地址
    "action":"https://iope.tmall.com/campaign-3734-0.htm"
}

```

##代码

### 采用通用 model，开发自定义 View

```
//注册组件
builder.registerCell(1, TestView.class);

//组件实现
public class TestView extends FrameLayout implements ITangramViewLifeCycle {
    private TextView textView;
    private BaseCell cell;

    public TestView(Context context) {
        super(context);
        init();
    }

    public TestView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init();
    }

    public TestView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init();
    }

    private void init(){
        inflate(getContext(), R.layout.item, this);
        textView = (TextView) findViewById(R.id.title);
    }

    @Override
    public void cellInited(BaseCell cell) {
        setOnClickListener(cell);
        this.cell = cell;
    }

    @Override
    public void postBindView(BaseCell cell) {
        int pos = cell.pos;
        textView.setText(
                cell.id + " pos: " + pos + " " + cell.parent.getClass().getSimpleName() + " " + cell
                        .optParam("msg"));

        if (pos > 57) {
            textView.setBackgroundColor(0x66cccf00 + (pos - 50) * 128);
        } else if (pos % 2 == 0) {
            textView.setBackgroundColor(0xaaaaff55);
        } else {
            textView.setBackgroundColor(0xcceeeeee);
        }
    }

    @Override
    public void postUnBindView(BaseCell cell) {
    }
}
```

### 采用自定义 model 和自定义 View

```
//注册组件
builder.registerCell(1,
                TestViewHolderCell.class, TextView.class));
                
//组件实现
public class TestViewHolderCell extends BaseCell<TextView> {

    @Override
    public void bindView(@NonNull TextView view) {
        TextView textView = view;
        textView.setText(
                id + " pos: " + pos + " " + parent.getClass().getSimpleName() + " " + optParam(
                        "msg"));

        if (pos > 57) {
            textView.setBackgroundColor(0x66cccf00 + (pos - 50) * 128);
        } else if (pos % 2 == 0) {
            textView.setBackgroundColor(0xaaaaff55);
        } else {
            textView.setBackgroundColor(0xccfafafa);
        }
    }

    @Override
    public void postBindView(@NonNull TextView view) {
        super.postBindView(view);
    }

    @Override
    public void unbindView(@NonNull TextView view) {
        super.unbindView(view);
    }
}
```