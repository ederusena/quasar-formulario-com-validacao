<template>
  <q-page
    class="container"
    padding
  >
    <p class="text-h4">Formulário</p>
    <form
      @submit.prevent.stop="onSubmit"
      @reset.prevent.stop="onReset"
      class="row q-col-gutter-sm"
    >
      <q-input
        class="col-md-12 col-sm-12 col-xs-12"
        clear-icon="clear"
        clearable
        color="orange-4"
        label="Nome"
        outlined
        placeholder="Nome"
        ref="nomeRef"
        :rules="nomeRules"
        v-model="form.nome"
      >
        <template v-slot:prepend>
          <q-icon name="person" />
        </template>
      </q-input>

      <q-input
        class="col-md-12 col-sm-12 col-xs-12"
        label="Idade"
        outlined
        ref="idadeRef"
        type="number"
        :rules="idadeRules"
        v-model="form.idade"
      >
        <template v-slot:prepend>
          <q-icon name="person" />
        </template>
      </q-input>
      <q-input
        class="col-md-12 col-sm-12 col-xs-12"
        label="Email"
        outlined
        ref="emailRef"
        suffix="@gmail.com"
        :rules="emailRules"
        type="email"
        v-model="form.email"
      >
        <template v-slot:prepend>
          <q-icon name="email" />
        </template>
      </q-input>
      <q-input
        class="col-md-12 col-sm-12 col-xs-12"
        label="Telefone"
        mask="(##) #####-####"
        outlined
        :rules="telefoneRules"
        ref="telefoneRef"
        unmasked-value
        v-model="form.phone"
      >
        <template v-slot:prepend>
          <q-icon name="phone" />
        </template>
      </q-input>
      <div class="col-12">
        <q-btn
          class="float-left"
          color="primary"
          label="Cadastrar"
          type="submit"
        ></q-btn>

        <q-btn
          class="float-right"
          color="secondary"
          label="Reset Form"
          type="reset"
        ></q-btn>
      </div>
    </form>
    <p>{{ form.nome }}</p>
    <p>{{ form.idade }}</p>
    <p>{{ form.email }}</p>
    <p>{{ form.phone }}</p>
  </q-page>
</template>

<script>
import { useQuasar } from 'quasar'
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'PageIndex',
  setup () {
    const $q = useQuasar()

    const accept = ref(false)

    const nome = ref(null)
    const nomeRef = ref(null)

    const idade = ref(null)
    const idadeRef = ref(null)

    const email = ref(null)
    const emailRef = ref(null)

    const telefone = ref(null)
    const telefoneRef = ref(null)

    return {
      nome,
      nomeRef,
      nomeRules: [
        val => !!val || 'Campo obrigatório',
        val => val.length > 5 || 'Nome invalido'
      ],
      idade,
      idadeRef,
      idadeRules: [
        val => !!val || 'Campo obrigatório',
        val => (val > 0 && val < 100) || 'Idade Invalida'
      ],
      email,
      emailRef,
      emailRules: [
        val => !!val || 'Campo obrigatório',
        val => /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(val) || 'Email invalido'
      ],
      telefone,
      telefoneRef,
      telefoneRules: [
        val => !!val || 'Campo obrigatório',
        val => (val.length >= 10 && val.length <= 11) || 'Numero telefone invalido'
      ],
      accept,
      onSubmit () {
        nomeRef.value.validate()
        idadeRef.value.validate()
        emailRef.value.validate()
        telefoneRef.value.validate()
        // validate all fields

        if (nomeRef.value.hasError || idadeRef.value.hasError || emailRef.value.hasError || telefoneRef.value.hasError) {
          $q.notify({
            message: 'Formulário não enviado',
            color: 'negative',
            icon: 'error'
          })
        } else {
          $q.notify({
            message: 'Formulário enviado com sucesso',
            color: 'positive',
            icon: 'done_all'
          })
        }
        this.onReset()
      },
      async onReset () {
        await this.resetForm()
        nomeRef.value.resetValidation()
        idadeRef.value.resetValidation()
        emailRef.value.resetValidation()
        telefoneRef.value.resetValidation()
      },
      async resetForm () {
        this.form = {
          nome: '',
          idade: null,
          email: '',
          phone: ''
        }
      }
    }
  },
  data () {
    return {
      form: {
        nome: '',
        idade: null,
        email: '',
        phone: ''
      }
    }
  }
})
</script>
