<template>
<div class="userCard">
  <div class="userCard-header">Карточка пользователя</div>
  <div class="userCard-content">
    <form @submit="checkForm">
      <div class="form-field" v-if="errors.length > 0">
        <ul class="error">
          <li v-for="err in errors" :key="err">{{ err }}</li>
        </ul>
      </div>
      <div class="form-field">
        <label>Имя:</label>
        <input type="text" name="name1" v-model.trim="name1"/>
      </div>
      <div class="form-field">
        <label>Фамилия:</label>
        <input type="text" name="name2" v-model.trim="name2"/>
      </div>
      <div class="form-field">
        <label>E-mail:</label>
        <input type="text" name="email" v-model.trim="email"/>
      </div>
      <div class="form-field">
        <label>Телефон:</label>
        <input type="text" name="phone" @input="onPhoneInput" v-model.trim="phone"/>
      </div>
      <div class="form-field">
        <label>Дата рождения:</label>
        <input type="text" name="datar" @input="onDateInput" v-model="datar" placeholder="DD.MM.YYYY" />
      </div>
      
      <div class="form-field">
        <label>Загрузить аватар:</label>
        <div v-show="imageUploaded" class="imageWrapper">
          <div id="imageholder"><img ref="avImage" src="#" alt="аватар" /></div>
          <button class="button" @click="clearImage">❌ Очистить</button><br />
        </div>
        <input ref="myfile" type="file" name="avatarfile" @change="onFileChange" />
      </div>

      <div class="form-field">
          <input type="submit" class="button button-primary" value="Сохранить">
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
      phone: '',
      datar: '',
      // ошибки проверки формы
      errors: [],
      imageUploaded: false,
    }
  },
  methods: {
    onPhoneInput(e) {
      // при вводе в поле "телефон" удаляем все символы, кроме цифр, тире и пробела
      this.phone = e.target.value.replace(/[^0-9\s-]/g, "");
    },
    onDateInput(e) {
      // в поле "дата рождение" разрешено вводить цифры и точку
      this.datar = e.target.value.replace(/[^0-9.]/g, "");
    },
    checkForm(e) {
      // регулярное выражение для проверки email
      // в сети их множество, и все работают не 100% точно
      // не стал изобретать тут что-то особенное, нам нужно лишь указать на возможную ошибку ввода пользователя
      // все равно адрес надо проверять еще и на сервере (например путем отправки письма с подтверждением)
      const reEmail = /^[.-\w+]+@[.-\w]+$/;
      // проверка даты соотв dd.mm.yyyy
      const reDatar = /^\d\d\.\d\d\.\d\d\d\d$/;
      // телефон может состоять из цифр, пробелов и тире
      const rePhone = /^[\d\s-]+$/;
      // regular expressions знаю хорошо из прошлого опыта по php :)

      e.preventDefault();
      this.errors = [];
      if(this.name1 === '') {
        this.errors.push('Укажите имя');
      }
      if(this.name2 === '') {
        this.errors.push('Укажите фамилию');
      }
      if(!reEmail.test(this.email)) {
        this.errors.push('Проверьте правильность ввода email');
      }
      if(!reDatar.test(this.datar)) {
        this.errors.push('Проверьте правильность ввода даты рождения');
        // здесь мы не проверяем действительность даты, если ввели например 11.11.1111
        // оставим это серверу (зависит от бизнес логики)
        // тут могут быть проверки на диапазон даты рождения и прочие
      }
      if(!rePhone.test(this.phone)) {
        this.errors.push('Проверьте правильность ввода номера телефона');
      }
      // todo: здесь можно проверять, загружен ли аватар

      if(this.errors.length === 0) {
        // тут можно отправить данные на сервер
        alert('Проверки пройдены');
      } else {
        alert('Обнаружены ошибки');
      }
    },

    // обработчик выбора кнопки "Обзор..." для загрузки файла
    onFileChange() {
      const inp = this.$refs.myfile;
      const img = this.$refs.avImage;
      const that = this;
      if(inp.files && inp.files[0]) {
        const reader = new FileReader();
        reader.onload = function(e) {
          img.src = e.target.result;
          that.imageUploaded = true;
        }
        reader.readAsDataURL(inp.files[0]);
      }
    },

    clearImage(e) {
      e.preventDefault();
      this.imageUploaded = false;
      this.$refs.avImage.src = null;
      this.$refs.myfile.value = null;
    },
    
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
  /* width: 100%; */
  font-size: 110%;
  border-radius: 5px;
  border: 1px solid #ccc;
  padding: 8px;
}

.button {
  color: #000;
  background-color: #ccc;
  border: 0px;
  padding: 10px;
  border-radius: 5px;
}

.button-primary {
  background-color: #41b883 !important;
  color: #fff !important;
}

.error {
  color: red;
}

.imageWrapper {
  margin-bottom: 8px;
}

#imageholder {
  width: 200px;
  height: 200px;
  background-color: #eee;
  border: 2px dashed #ccc;
}

#imageholder img { width: 100%; }

</style>
