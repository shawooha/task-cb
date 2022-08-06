<template>
<div class="userCard">
  <div class="userCard-header">Карточка пользователя</div>
  <div class="userCard-content">
    <form>
      <div class="form-field">
        <label>Имя:</label>
        <input type="text" name="name1" v-model="name1"/>
      </div>
      <div class="form-field">
        <label>Фамилия:</label>
        <input type="text" name="name2" v-model="name2"/>
      </div>
      <div class="form-field">
        <label>E-mail:</label>
        <input type="text" name="email" v-model="email"/>
      </div>
      <div class="form-field">
        <label>Дата рождения:</label>
        <input type="text" name="datar" v-model="datar" placeholder="DD.MM.YYYY" />
      </div>
      
      <div class="form-field">
        <label>Загрузить аватар:</label>
        <p>TODO</p>
      </div>

      <div class="form-field">
          <input type="submit" class="button" value="Сохранить" @click="onClick">
      </div>
    </form>
  </div>
</div>
</template>

<script>
export default {
  name: 'UserCard',
  data() {
    return {
      name1: '',
      name2: '',
      email: '',
      datar: '',
    }
  },
  methods: {
    onClick: function(e) {
      e.preventDefault();
      alert(this.name1 + this.name2)
    }
  }
}
</script>

<style scoped>
.userCard {
  width: 600px;
  border: 1px solid #ccc;
}

.userCard-header {
  background-color: #41b883;
  color: #fff;
  padding: 16px;
  font-size: 150%;
  font-weight: bold;
}

.userCard-content {
  padding: 20px;
}

.form-field {
  margin-bottom: 16px;
}

.form-field label {
  display: block;
  font-weight: bold;
}

input[type=text] {
  box-sizing: border-box;
  width: 100%;
  font-size: 110%;
  border-radius: 5px;
  border: 1px solid #ccc;
  padding: 8px;
}

.button {
  background-color: #41b883;
  color: #fff;
  font-size: 120%;
  border: 0px;
  padding: 10px;
  border-radius: 5px;
}

.button:hover {
  color: #ddd;
}
</style>
