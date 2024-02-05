<script setup lang="ts">
import { NInput, NButton, NModal, NCard } from 'naive-ui';
import { getYearString } from 'naive-ui/es/date-picker/src/utils';
import { ref } from 'vue';
import { defineProps } from 'vue';

interface User {
  username: string;
  passwd: string;
}

const openLoginModal = ref(false);
const openSignupModal = ref(false);
const loggedIn = ref(false);
const enteredUsername = ref('');
const enteredPasswd = ref('');

const userList = ref<User[]>([
  { username: 'giang', passwd: '123456' },
  { username: 'henry', passwd: 'abcde' },
  { username: 'cindy', passwd: 'xyzabc' }
]);

const handleLogin = function (user: User) {
  const currUser = userList.value.find(
    (singleUser) =>
      singleUser.username === user.username && singleUser.passwd === user.passwd
  );
  if (currUser) {
    console.log('signed in');
    loggedIn.value = true;
    openLoginModal.value = false;
  } else {
    alert('Error. Please review your username/password!');
  }
};

const handleSignUp = function (user: User) {
  if (
    !userList.value.find(
      (currUser) =>
        user.username === currUser.username && user.passwd === currUser.passwd
    )
  ) {
    const newUser = { username: user.username, passwd: user.passwd };
    userList.value.push(newUser);
    loggedIn.value = true;
    openSignupModal.value = false;
  } else {
    alert('You already signed up. Please log into your account!');
  }
};

const handleLogout = function () {
  loggedIn.value = false;
};
</script>
<template>
  <div class="buttons">
    {{ enteredUsername }}
    <div v-if="!loggedIn">
      <NButton style="teritory" @click="openLoginModal = true">Login</NButton>
      <NModal v-model:show="openLoginModal">
        <NCard
          style="width: 600px"
          :bordered="false"
          size="huge"
          title="Log in"
          role="dialog"
          aria-modal="true"
        >
          <div class="login-details">
            <div class="login-form">
              <div class="usrname-field">
                <label>Username:</label>
                <NInput
                  type="text"
                  placeholder="Enter username"
                  required
                  v-model="enteredUsername"
                ></NInput>
              </div>
              <div class="passwd-field">
                <label>Password:</label>
                <NInput
                  type="password"
                  placeholder="Enter password"
                  show-password-on="click"
                  v-model="enteredPasswd"
                  required
                ></NInput>
              </div>
              <NButton type="primary" @click="handleLogin">Submit</NButton>
            </div>
          </div>
        </NCard>
      </NModal>
    </div>
    <div v-if="!loggedIn">
      <NButton style="teritory" @click="openSignupModal = true">
        Sign up
      </NButton>
      <NModal v-model:show="openSignupModal">
        <NCard
          style="width: 600px"
          :bordered="false"
          title="Sign up"
          size="huge"
          role="dialog"
          aria-modal="true"
        >
          <div class="login-details">
            <div class="login-form">
              <div class="usrname-field">
                <label>Username:</label>
                <NInput
                  type="text"
                  placeholder="Enter username"
                  required
                  v-model="enteredUsername"
                ></NInput>
              </div>
              <div class="passwd-field">
                <label>Password:</label>
                <NInput
                  type="password"
                  placeholder="Enter password"
                  show-password-on="click"
                  v-model="enteredPasswd"
                  required
                ></NInput>
              </div>
              <NButton type="primary" @click="handleSignUp">Submit</NButton>
            </div>
          </div>
        </NCard>
      </NModal>
    </div>
    <div v-else><NButton @click="handleLogout">Log out</NButton></div>
  </div>
</template>

<style>
.login-form {
  justify-content: center;
  align-items: center;
}

.login-details {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 24px;
}

.login-form {
  justify-content: left;
  gap: 10px;
  align-items: flex-start;
  display: flex;
  flex-direction: column;
  flex: 1;
}
.passwd-field {
  flex: 1;
  width: 100%;
}
.usrname-field {
  flex: 1;
  width: 100%;
}
.buttons {
  display: flex;
}
</style>
