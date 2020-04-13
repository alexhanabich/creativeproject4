<template>
<div id="app">
  <div class="header">
    <div class="title">
      <router-link to="/">
        <h1><i class="fas fa-utensils fa-2x"></i> What Should I Have for Dinner?</h1>
      </router-link>
    </div>
  </div>
  <div class="content">
    <router-view />
  </div>
  <div class="footer">
    <router-link to="/admin">Setting</router-link>
    <div class="github">
      <a href = "https://github.com/andhim/creativeproject4" style = "color: black;"><i class="fab fa-github"></i></a>
    </div>
  </div>
</div>
</template>

<style>

a {
  text-decoration: none;
  color: black;
}
html {
  box-sizing: border-box;
}

body {
  font-family: 'Indie Flower', cursive;

  font-size: 16px;
  background: #fff;
  padding: 0px;
  margin: 0px;
}

/* Header */
.header {
  display: flex;
  justify-content: center;
  padding: 10px 0px 0px 0px;
  background-color: #5BDEFF;
  color: #1C454F;
  width: 100%;
}

.title {
  margin-top: 5px;
  display: flex;
  justify-content: center;
  text-decoration: none;
}

.title h1 {
  font-size: 30px;
}

.content {
  min-height: 500px;
}

/* Footer */
.footer {
  height: 50px;
  padding: 20px 0px 0px 0px;
  background: #e3e3e3;
  font-size: 12px;
  display: flex;
  flex-direction: row;
  width: 100%;
}

.footer a {
  color: #000;
}

h1 {
  font-size: 20px;
  text-decoration: none;
}

h2 {
  font-size: 14px;
}

.github {
  font-size: 30px;
  margin-left: 30px;
}
</style>
