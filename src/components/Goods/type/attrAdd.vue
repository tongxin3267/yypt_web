  <template>
    <div class="m-l-50 m-t-30 w-500">
		<el-form :model="form" label-width="110px">
			<el-form-item label="属性名称" prop="name">
				<el-input v-model.trim="form.name" class="h-40 w-200"></el-input>
			</el-form-item>
            <el-form-item label="属性模型">
                  <el-select v-model="form.type_id" placeholder="请选择">
                    <el-option
                        v-for="item in types"
                        :key="item.id"
                        :label="item.name"
                        :value="item.id">
                    </el-option>
                </el-select>
            </el-form-item>
            <el-form-item label="能否检索">
                <el-radio v-model="form.attr_index" label="0">否</el-radio>
                <el-radio v-model="form.attr_index" label="1">是</el-radio>
            </el-form-item>
            <el-form-item label="属性值">
                <el-radio v-model="form.attr_type" label="0">唯一属性</el-radio>
                <el-radio v-model="form.attr_type" label="1">单选属性</el-radio>
                <el-radio v-model="form.attr_type" label="2">多选属性</el-radio>
            </el-form-item> 
            <el-form-item label="属性值录入方式">
                <el-radio v-model="form.input_type" label="0">手工录入</el-radio>
                <el-radio v-model="form.input_type" label="1">从下面的列表中选择</el-radio>
                <el-radio v-model="form.input_type" label="2">多行文本框</el-radio>
            </el-form-item>
            <el-form-item label="可选值">
                <el-input type="textarea" v-model="form.values" placeholder="请填写可选值，例如： A|B|C|D"></el-input>
            </el-form-item>
            <el-form-item label="排序">
				<el-input v-model.trim="form.sort" class="h-40 w-200"></el-input>
			</el-form-item>
			<el-form-item>
				<el-button type="primary" @click="save('form')">提交</el-button>
				<el-button @click="goback()">返回</el-button>
			</el-form-item>
		</el-form>
	</div>   
  </template>
  <script>
  import http from '../../../assets/js/http'
  import fomrMixin from '../../../assets/js/form_com'

  export default {
    data() {
        return {
            form:{
                id: '',
                name : '',
                type_id : '',
                attr_index : '',
                attr_type : '',
                input_type : '',
                values : '',
                sort : 50,
            },
            types :[],
        }
    },
    created() {
        this.apiGet("admin/type/editAttr?id=0").then((res) => {
            _g.closeGlobalLoading();
            this.types = res.data.types;
        });
    },
    methods:{
        save(){
            this.apiPost("admin/type/saveAttr", this.form).then((res)=>{
                this.handelResponse(res, (data) => {
                    _g.toastMsg('success', '修改成功');
                    setTimeout(this.goback(), 1500);
                })
            });
        }
    },
    mixins: [http, fomrMixin]
  }
  </script>
