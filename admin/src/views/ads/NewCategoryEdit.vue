<template>
  <div class="about">
    <h1> {{id? '修改' : '新建'}}文章分类</h1>
    <el-form label-width="120px" @submit.native.prevent="saveform('model')" :model="model" :rules="rules" ref="model">
      <el-form-item label="分类名称" prop="name">
        <el-col :span="4">
          <el-input v-model="model.name" input-width="240px" clearable></el-input>
        </el-col>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" native-type="submit">保存</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
  export default {
    props: {
      id: {}
    },
    data() {
      return {
        model: {},
        rules: {
          name: [{
              required: true,
              message: '请输入分类名称',
              trigger: 'blur'
            },
            {
              min: 1,
              max: 6,
              message: '请输入小于6个字',
              trigger: 'change'
            }
          ],
        }
      }
    },
    methods: {
      saveform(model) {
        this.$refs[model].validate(valid => {
          if (valid) {
            this.save();
          } else {
            console.log('error submit!!')
            return false
          }
        })
      },
      async save() {
        let res
        if (this.id) {
          res = this.$http.put(`article/article_categories/${this.id}`, this.model)
        } else {
          res = this.$http.post('article/article_categories', this.model)
        }
        res = await this.$http.get('article/article_categories')
        this.$router.push('/article_cat/list')
        this.$message({
          type: 'success',
          message: '保存成功'
        })
      },
      async fetch() {
        const res = await this.$http.get(`article/article_categories/${this.id}`)
        this.model = res.data
      }
    },
    created() {
      this.id && this.fetch()
    }
  }
</script>