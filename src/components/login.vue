<template>
<div class="login">
  <br><br>
  <Row>
    <Col span="22" offset="1">
        <Alert show-icon>登录窗口
         <span slot="desc">成功的提示描述文案成功的提示描述文案成功的提示描述文案成功的提示描述文案成功的提示描述文案</span></Alert>
     </Alert>
    </Col>
  </Row><br>
  <Row>
    <Col span="22" offset="1">
    <Input type="text" size="large" placeholder="用户名" icon="person" required="required" ref='user' name="username" v-model="userName" :value='user_name' /></Input>
    </Col>
  </Row><br>
  <Row>
    <Col span="22" offset="1">
    <Input type="password" size="large" placeholder="密码" icon="locked" required="required" ref='pwd' name="pwd" v-model="pwd" /></Input>
    </Col>
  </Row><br>
  <Row>
    <Col span="10" offset="1">
    <router-link to="/register">
      <Button long shape="circle">注册</Button>
    </router-link>
    </Col>
    <Col span="10" offset="2">
    <Button @click="checkUser" shape="circle" long type="primary">登录</Button>
    </Col>

  </Row>
  <input class='check_btn' type="hidden" id="chkRememberPass" name="chkRememberPass" checked="">
  <div class='footer'>
    <router-link to="/about"><span>关于我</span></router-link>|
    <span><router-link to="/diary/d_edit">发表动态</router-link></span>|
    <span><router-link to="/message_board/m_edit">給我留言</router-link></span>
  </div>
  <Modal v-model="modal1" title="登录成功" @on-ok="ok" @on-cancel="cancel">
    <span>{{notice_info}}</span>
    <span><router-link to="/diary/d_edit"><span>发表动态</span></router-link><span>|</span>
    <span><router-link to="/message_board/m_edit">给我</router-link></span></span>
  </Modal>
</div>
</template>
<script>
import {  mapState} from 'vuex'
import {  setCookie} from '../js/setcookie.js'
import axios from 'axios'
import '../js/init.js'
export default {
  computed: mapState({
    user_name: state => state.user_name
  }),
  data() {
    return {
      userName: '',
      pwd: '',
      modal1: false,
      notice_info: '用户名密码错误或不存在！'
    }
  },
  created() {
     //this.userName = unescape(setCookie.setInfo().name);
     //this.pwd = setCookie.setInfo().mypwd;
  },
  methods: {
    ok() {
      this.$Message.info('点击了确定');
    },
    cancel() {
      this.$Message.info('点击了取消');
    },
    checkUser() {
      var name = this.userName;
      var pwd = this.pwd;
      var self = this; //很关键
      if (name != '' && pwd != '') {
        axios({
          method: 'get',
          url: 'api/user/searchUser',
          data: {
            username: name,
            pwd: pwd
          },
          timeout: 3000
        }).then(function(response) {
          var i, flag;
          for (i in response.data) {
            if (name == response.data[i].user_name && pwd == response.data[i].user_pwd && name != '' && pwd != '') {
              flag = 'allright';
            }
          }
          if (flag == 'allright') {
            var cookie_name = unescape(setCookie.setInfo().name);
            if ((cookie_name != name && name != '' && cookie_name != '') || cookie_name == '') {
              setCookie.getInfo(self.userName, self.pwd);
              setCookie.userLogin();
              self.notice_info = '登录成功！';
              self.$Message.success({
                content: self.notice_info
              });
              self.modal1 = true
            } else if (cookie_name == name && name != '' && cookie_name != '') {
              self.notice_info = '登录成功';
              self.$Message.success({
                content: self.notice_info
              });
              self.modal1 = true
            }
          } else {
            self.$Message.error({
              content: self.notice_info
            });
          }
        }).catch(function(error) {
          console.log(error);
          self.notice_info = '服務器繁忙，請刷新頁面或者稍後重試!(Error code: 504)';
          self.$Message.error({
            content: self.notice_info
          });
        });
      } else {
        self.$Message.error({
          content: self.notice_info
        });
      }
    }
  }
}
//使用scoped属性可以保证样式只在该组件中起作用
</script>
<style scoped src="../assets/css/login.css"></style>
