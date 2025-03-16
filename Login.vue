<template>

  
    <div class="login">
			<h1>Login</h1>
			<div class="write">
               <label for="user">用户名</label>
		
			<input type="text" id="user" name="user" placeholder="Username or Emall"  />
			
			<label for="password">密码</label>
			<input type="password" id="password" name="password" placeholder="Password"  />
			</div>
			<div class="botton">LOGIN</div>
	</div>
</template>
<script setup lang="ts">
 
</script>
<style scoped>
.login{
			width: 40em;
			height: 40em;
			
			box-shadow: 3px 4px 10px rgba(23, 19, 19, 0.3);
			
			margin:0 auto;
			
}
		h1{ 
			padding-top: 30px;
			text-align: center;
			margin-top: 1em;
			letter-spacing: 3px;
			color: #0b0b0b;
			font-size: 3em;
		}
input{
			width: 25em;
			height: 4em;
			margin-left: 10em;
			margin-top: 10px;
			margin-bottom:2em ;
			border: 3px solid #000000;
		
			padding-left: 10px;
			
		}
		input:focus{
			outline: none;
			border: 5px dashed #0b0b0b;
		}
		
		label{
			padding-top: 10px;
			display: block;
			margin-left: 5em;
			margin-top: 10px;
			font-size: 1em;
			color: #0b0b0b;
			font-weight: bold;
		}
		.botton{
			width: 15em;
			height: 4em;
			margin-left: 13em;
			margin-top: 2em;
			margin-bottom:2em ;
			border: 1px solid #000000;
			border-radius: 5px;
			padding-left: 10px;
			background-color: #0b0b0b;
			color: #ffffff;
			font-weight: bold;
			cursor: pointer;
			line-height: 4em;
			text-align: center;
			transition: all 0.3s ease;
		}
		
		.botton:active{
			background-color: #e8e4e4;
			color: #1c1818;
		}
	  .write{
	  	
	  	width:20em ;
	  	
	  }
	  .password{
		margin-bottom: 10px;
	  }
</style>