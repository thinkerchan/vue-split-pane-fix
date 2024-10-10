# vue-splitpane-fix
> base on https://github.com/PanJiaChen/vue-split-pane/tree/master


### [try demo](http://thinkerchan.github.io/vue-splitpane-fix/demo/index.html)

### How to use?
```bash
npm install vue-splitpane-fix

#import
import splitPane from 'vue-splitpane-fix'

# use as global component
Vue.component('split-pane', splitPane);
```


### Options
|    Property    |    Description   |   type   |	default	|
| -----------------  | ---------------- | :--------: | :----------: |
| split       | the split type |String [horizontal,vertical] |must choose one type |
| default-percent         | paneL default-percent  |Number | 50 |
| min-percent         | paneL max-min-percent  |Number | 10 |
| default-px         | paneL default-px  |Number | 0 |
| min-px         | paneL min-px  |Number | 300 |

> if you set `default-px` with a non-zero value, `default-percent` will not work
