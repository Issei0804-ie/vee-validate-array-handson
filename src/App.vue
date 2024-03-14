<script setup lang="ts">
import { useFieldArray, useForm } from 'vee-validate'
import * as yup from 'yup'

const { errors } = useForm({
  validationSchema: yup.object({
    objects: yup.array().of(
      yup.object({
        name: yup.string().required(),
        description: yup.string().required()
      })
    )
  }),
  initialValues: {
    objects: [{ name: '', description: '' }]
  }
})

const { fields, push } = useFieldArray<{ name: string; description: string }>('objects')
</script>

<template>
  <div>
    <form>
      <div v-for="(field, index) in fields" :key="field.key">
        <div class="name">
          <input v-model="field.value.name" />
          <span class="show-error"> {{ errors[`objects[${index}].name`] }}</span>
          <span class="not-show-error"> {{ errors[`objects.${index}.name`] }}</span>
        </div>

        <div class="description">
          <input v-model="field.value.description" />
          <span class="show-error"> {{ errors[`objects[${index}].description`] }}</span>
          <span class="not-show-error"> {{ errors[`objects.${index}.description`] }}</span>
        </div>
      </div>
      <button type="button" @click="push({ name: '', description: '' })">
        Click to Push object
      </button>
    </form>
  </div>
</template>
