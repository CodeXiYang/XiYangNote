# CSS补充说明

## 1. 利用伪元素选择器修改checkbox的样式

```html
<label><input type="checkbox">身份证</label>
```

```css
        input[type="checkbox"] {
            width: 13px;
            height: 13px;
            display: inline-block;
            text-align: center;
            vertical-align: middle;
            line-height: 18px;
            margin-right: 10px;
            position: relative;
        }

        input[type="checkbox"]::before {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            background: #fff;
            width: 100%;
            height: 100%;
            border: 1px solid red;
            border-radius: 3px;
        }

        input[type="checkbox"]:checked::before {
            content: "✔️";
            background-color: red;
            position: absolute;
            top: 0;
            left: 0;
            width: 117%;
            border: 1px solid red;
            border-radius:3px;
            color: green;
            font-size: 10px;
            font-weight: bold;
        }
```