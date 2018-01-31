<template>
	<div class="left-nav">
		<ul class="leftNavList">
			<li>
				<img src="../assets/tree.png">
				<div>收银</div>				
			</li>
			<li>
				<img src="../assets/tree.png">
				<div>收银</div>				
			</li>
			<li>
				<img src="../assets/tree.png">
				<div>收银</div>				
			</li>
			<li>
				<img src="../assets/tree.png">
				<div>收银</div>				
			</li>
		</ul>
	</div>
</template>
<script>
	export default {
	   name: 'leftNav',
	   data () {
	   	  return {

	   	  }
	   }
	}
</script>
<style lang="scss" scoped>
	@import "src/style/mixin";
.left-nav{
   color:#fff;
   font-size:10px;
  @include width_height(5%,100vh);
  @include bg_color;
   float:left;
   ul{
    padding:0px;
    margin: 0px;
    	li{
		    list-style: none;
		    text-align: center;
		    border-bottom:1px solid #20a0ff;
		    padding:10px;
		      img{
	             @include width_height(24px,24px);
			  }
		 }
	}
 
}

</style>