# vue-bootstrap-collapse

Vue2 wrapper for Bootstrap's collapse component


## Basic Usage
Inside your vue js component:
```
<template>
   <collapse :visible="true"></collapse>
</template>
<script>
import collapse from 'vue-bootstrap-collapse'
export default {
  components: {
    collapse
  },
  //...
}
</script>
```
## Events
Standard bootstrap events are emitted:

* collapse-show (alias of show.bs.collapse)
* collapse-shown (alias of shown.bs.collapse)
* collapse-hide (alias of hide.bs.collapse)
* collapse-hidden (alias of hidden.bs.collapse)

```
<collapse
	@collapse-show="handleExpandStart"
	@collapse-shown="handleExpandComplete"
	@collapse-hide="handleCollapseStart"
	@collapse-hidden="handleCollapseComplete">
</collapse>
```
