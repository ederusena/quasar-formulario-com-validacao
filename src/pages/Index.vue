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

      <q-select
        outlined
        v-model="form.tipoPessoa"
        :options="optionsPessoa"
        ref="tipoPessoaRef"
        label="Tipo de Pessoa"
        :rules="tipoPessoaRules"
        class="col-md-12 col-sm-12 col-xs-12"
        emit-value
        map-options
      />

      <span class="text-bold">Sexo:</span>
      <q-option-group
        :options="optionsSexo"
        label="Sexo"
        type="radio"
        v-model="form.sexo"
        class="col-md-12 col-sm-12 col-xs-12"
      />

    <span class="text-bold">Possui alguma dificuldade? </span>
      <q-option-group
        :options="optionsDificuldades"
        label="Dificuldades"
        type="checkbox"
        v-model="form.dificuldades"
        class="col-md-12 col-sm-12 col-xs-12"
      />

      <q-toggle
        v-model="form.notificacoes"
        color="primary"
        icon="mail"
        label="Receber notificações?"
        size="lg"
      />

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
    <p>{{ form.tipoPessoa }}</p>
  </q-page>
</template>

<script>
import { useQuasar } from 'quasar'
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'PageIndex',
  setup () {
    const $q = useQuasar()
    const nomeRef = ref(null)
    const idadeRef = ref(null)
    const emailRef = ref(null)
    const telefoneRef = ref(null)
    const tipoPessoaRef = ref(null)

    return {
      nomeRef,
      nomeRules: [
        val => !!val || 'Campo obrigatório',
        val => val.length > 5 || 'Nome invalido'
      ],
      idadeRef,
      idadeRules: [
        val => !!val || 'Campo obrigatório',
        val => (val > 0 && val < 100) || 'Idade Invalida'
      ],
      emailRef,
      emailRules: [
        val => !!val || 'Campo obrigatório',
        val => /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(val) || 'Email invalido'
      ],
      telefoneRef,
      telefoneRules: [
        val => !!val || 'Campo obrigatório',
        val => (val.length >= 10 && val.length <= 11) || 'Numero telefone invalido'
      ],
      tipoPessoaRef,
      tipoPessoaRules: [
        val => !!val || 'Campo obrigatório'
      ],
      onSubmit () {
        nomeRef.value.validate()
        idadeRef.value.validate()
        emailRef.value.validate()
        telefoneRef.value.validate()
        // validate all fields

        if (nomeRef.value.hasError || idadeRef.value.hasError || emailRef.value.hasError || telefoneRef.value.hasError || tipoPessoaRef.value.hasError) {
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
        tipoPessoaRef.value.resetValidation()
      },
      async resetForm () {
        this.form = {
          nome: '',
          idade: null,
          email: '',
          phone: '',
          tipoPessoa: '',
          sexo: 'NI',
          dificuldades: [],
          notificacoes: false
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
        phone: '',
        tipoPessoa: '',
        sexo: 'NI',
        dificuldades: [],
        notificacoes: false
      },
      optionsPessoa: [
        { label: 'Pessoa Fisica', value: 'PF' },
        { label: 'Pessoa Juridica', value: 'PJ' }
      ],
      optionsSexo: [
        { label: 'Masculino', value: 'M' },
        { label: 'Feminino', value: 'F' },
        { label: 'Não informado', value: 'NI' }
      ],
      optionsDificuldades: [
        { label: 'Motoras', value: 'motoras' },
        { label: 'Visuais', value: 'visuais' },
        { label: 'Respiratórias', value: 'respiratorias' }
      ]
    }
  }
})
</script>
