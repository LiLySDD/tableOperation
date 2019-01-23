<template>
    <el-container style="height: calc(100vh - 50px);min-width:1000px;width: calc(100vw - 200px) ;border: 0;padding: 0;background-color: bisque;flex-direction: column">
        <div >
            <el-form ref="look" :model="look" size="mini">
                <el-row :gutter="10" style="display:flex;flex-direction: row;flex-wrap:wrap;margin-left: 0;margin-right: 0;">
                    <el-form-item  style="width: 210px;" label="工号：">
                        <el-input placeholder="请输入内容" v-model="look.id" clearable></el-input>
                    </el-form-item>
                    <el-form-item style="width: 210px;"  label="姓名：">
                        <el-input placeholder="请输入内容" v-model="look.name" clearable></el-input>
                    </el-form-item>
                    <el-form-item style="width: 210px;"  label="性别：">
                        <el-radio-group v-model="look.gender">
                            <el-radio label="男"></el-radio>
                            <el-radio label="女"></el-radio>
                        </el-radio-group>
                    </el-form-item>
                    <el-form-item style="width: 210px;height: 26px;" label="城市:">
                        <el-select style="width: 150px;"  v-model="look.region" placeholder="请选择城市">
                            <el-option label="上海" value="上海"></el-option>
                            <el-option label="北京" value="北京"></el-option>
                            <el-option label="大连" value="大连"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item style="width: 270px;"  label="身份证号：">
                        <el-input placeholder="请输入内容" v-model="look.identifier" clearable></el-input>
                    </el-form-item>
                    <el-form-item style="width: 380px;"  label="日期：">
                        <el-date-picker
                                v-model="look.date"
                                style="width: 320px;height: 26px;"
                                type="daterange"
                                start-placeholder="开始日期"
                                end-placeholder="结束日期"
                                value-format="yyyy-MM-dd">
                        </el-date-picker>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" size="mini" @click="dialogFormVisible = true">新建</el-button>
                        <el-button type="primary" size="mini">查询</el-button>
                    </el-form-item>
                </el-row>
            </el-form>
            <el-dialog title="添加" :visible.sync="dialogFormVisible" :before-close="handleClose" :close-on-press-escape="false" width="28%" center>
                <el-form :model="form" status-icon :rules="rules" ref="form" label-width="100px" class="demo-ruleForm">
                    <el-form-item label="工号" prop="id">
                        <el-input style="width: 320px;"  v-model="form.id"></el-input>
                    </el-form-item>
                    <el-form-item label="姓名" prop="name">
                        <el-input style="width: 320px;" v-model="form.name"></el-input>
                    </el-form-item>
                    <el-form-item style="width: 320px;" label="性别" prop="gender">
                        <el-radio-group v-model="form.gender">
                            <el-radio label="男"></el-radio>
                            <el-radio label="女"></el-radio>
                        </el-radio-group>
                    </el-form-item>
                    <el-form-item label="城市" prop="region">
                        <el-select style="width: 320px;" v-model="form.region" placeholder="请选择城市">
                            <el-option label="上海" value="上海"></el-option>
                            <el-option label="北京" value="北京"></el-option>
                            <el-option label="大连" value="大连"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item label="身份证号" prop="identifier">
                        <el-input style="width: 320px;" v-model="form.identifier"></el-input>
                    </el-form-item>
                    <el-form-item label="日期" prop="date">
                        <el-date-picker
                                v-model="form.date"
                                type="daterange"
                                size="mini"
                                style="width: 320px;"
                                start-placeholder="开始日期"
                                end-placeholder="结束日期"
                                value-format="yyyy-MM-dd">
                        </el-date-picker>
                    </el-form-item>
                </el-form>
                <div slot="footer" class="dialog-footer">
                    <el-button type="primary" @click="submitForm('form')">创建</el-button>
                    <el-button @click="resetForm('form')">重置</el-button>
                    <el-button @click="cc('form')">取 消</el-button>
                </div>
            </el-dialog>
        </div>
        <div>
            <el-form :model="checkForm" status-icon :rules="rules" ref="checkForm" class="demo-ruleForm">
            <el-table
                    border
                    stripe
                    :data="tableData"
                    size="mini"
            >
                <el-table-column
                        prop="id"
                        label="工号"
                >
                    <template scope="scope">
                        <el-input size="mini" style="height: 100%" v-show="scope.row.id===''" v-model="checkForm.id" :disabled="true"></el-input>
                        <span v-show="scope.row.id!==''">{{ scope.row.id }}</span>
                    </template>
                </el-table-column>
                <el-table-column
                        prop="name"
                        label="姓名"
                >
                    <template scope="scope">
                            <el-form-item prop="name">
                                <el-input size="mini" style="height: 100%" v-show="scope.row.name===''" v-model="checkForm.name"></el-input>
                                <span v-show="scope.row.name!==''">{{ scope.row.name }}</span>
                            </el-form-item>
                    </template>
                </el-table-column>
                <el-table-column
                        prop="gender"
                        label="性别"
                >
                    <template scope="scope">
                            <el-form-item prop="gender">
                                <el-radio-group v-show="scope.row.gender===''" v-model="checkForm.gender">
                                    <el-radio label="男"></el-radio>
                                    <el-radio label="女"></el-radio>
                                </el-radio-group>
                                <span v-show="scope.row.gender!==''">{{ scope.row.gender }}</span>
                            </el-form-item>
                    </template>
                </el-table-column>
                <el-table-column
                        prop="date1"
                        label="开始日期"
                >
                    <template scope="scope">
                            <el-form-item prop="date[0]"  :rules="rules1">
                                <el-date-picker
                                        v-model="checkForm.date[0]"
                                        v-show="scope.row.date1===''"
                                        style="height: 100%"
                                        size="mini"
                                        type="date"
                                        placeholder="开始日期"
                                        value-format="yyyy-MM-dd">
                                </el-date-picker>
                                <span v-show="scope.row.date1!==''">{{ scope.row.date1 }}</span>
                            </el-form-item>
                    </template>
                </el-table-column>
                <el-table-column
                        prop="date2"
                        label="结束日期"
                >
                    <template scope="scope">
                            <el-form-item prop="date[1]" :rules="[
                            { required: true, message: '请输入结束日期',trigger: 'blur'}]">
                                <el-date-picker
                                        v-model="checkForm.date[1]"
                                        v-show="scope.row.date2===''"
                                        style="height: 100%"
                                        size="mini"
                                        type="date"
                                        placeholder="结束日期"
                                        value-format="yyyy-MM-dd">
                                </el-date-picker>
                                <span v-show="scope.row.date2!==''">{{ scope.row.date2 }}</span>
                            </el-form-item>
                    </template>
                </el-table-column>
                <el-table-column
                        prop="region"
                        label="城市"
                >
                    <template scope="scope">
                            <el-form-item prop="region">
                                <el-select v-show="scope.row.region===''" v-model="checkForm.region" placeholder="请选择城市">
                                    <el-option label="上海" value="上海"></el-option>
                                    <el-option label="北京" value="北京"></el-option>
                                    <el-option label="大连" value="大连"></el-option>
                                </el-select>
                                <span v-show="scope.row.region!==''">{{ scope.row.region }}</span>
                            </el-form-item>
                    </template>
                </el-table-column>
                <el-table-column
                        prop="identifier"
                        label="身份证号"
                >
                    <template scope="scope">
                            <el-form-item prop="identifier">
                                <el-input size="mini" style="height: 100%" v-show="scope.row.identifier===''" v-model="checkForm.identifier"></el-input>
                                <span v-show="scope.row.identifier!==''">{{ scope.row.identifier }}</span>
                            </el-form-item>
                    </template>
                </el-table-column>
                <el-table-column
                        label="操作">
                    <template slot-scope="scope">
                        <div v-if="scope.row.id!==''">
                            <el-button
                                    @click="deleteRow(scope.$index, tableData)"
                                    type="danger"
                                    icon="el-icon-delete"
                                    size="mini" plain>
                            </el-button>
                            <el-button
                                    @click="handleEdit(scope.$index, scope.row)"
                                    icon="el-icon-edit"
                                    size="mini">
                            </el-button>
                        </div>
                        <div v-else>
                            <el-button type="success" icon="el-icon-success" @click="changeForm('checkForm')" size="mini" plain></el-button>
                            <el-button type="danger" icon="el-icon-error" @click="abolishChange(scope.$index, scope.row)" size="mini" plain></el-button>
                        </div>
                    </template>
                </el-table-column>
            </el-table>
            </el-form>
            <el-dialog title="修改" :visible.sync="dialogFormVisibleTwo" :close-on-press-escape="false" width="28%" center>
                <el-form :model="checkForm" status-icon :rules="rules" ref="checkForm" label-width="100px" class="demo-ruleForm">
                    <el-form-item label="工号" prop="id">
                        <el-input style="width: 320px;"  v-model="checkForm.id" :disabled="true"></el-input>
                    </el-form-item>
                    <el-form-item label="姓名" prop="name">
                        <el-input style="width: 320px;" v-model="checkForm.name"></el-input>
                    </el-form-item>
                    <el-form-item style="width: 320px;" label="性别" prop="gender">
                        <el-radio-group v-model="checkForm.gender">
                            <el-radio label="男"></el-radio>
                            <el-radio label="女"></el-radio>
                        </el-radio-group>
                    </el-form-item>
                    <el-form-item label="城市" prop="region">
                        <el-select style="width: 320px;" v-model="checkForm.region" placeholder="请选择城市">
                            <el-option label="上海" value="上海"></el-option>
                            <el-option label="北京" value="北京"></el-option>
                            <el-option label="大连" value="大连"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item label="身份证号" prop="identifier">
                        <el-input style="width: 320px;" v-model="checkForm.identifier"></el-input>
                    </el-form-item>
                    <el-form-item label="日期" prop="date">
                        <el-date-picker
                                v-model="checkForm.date"
                                type="daterange"
                                size="mini"
                                style="width: 320px;"
                                start-placeholder="开始日期"
                                end-placeholder="结束日期"
                                value-format="yyyy-MM-dd">
                        </el-date-picker>
                    </el-form-item>
                </el-form>
                <div slot="footer" class="dialog-footer">
                    <el-button type="primary" @click="changeForm('checkForm')">完成</el-button>
                    <el-button @click="dialogFormVisibleTwo = false">取 消</el-button>
                </div>
            </el-dialog>
        </div>
    </el-container>
</template>

<script>
export default {
    name: 'HelloWorld',
    props: {
        msg: String
    },
    data(){
        let dateTime = (rule, value, callback) => {
            if (!value) {
                return callback(new Error('日期不能为空'));
            }
            setTimeout(() => {
                if (!value) {
                    return callback(new Error('日期不能为空'));
                } else {
                    callback();
                }
            }, 1000);
        };
        return {
            form: {
                id: '',
                name: '',
                gender: '',
                identifier: '',
                date: [],
                region: ''
            },
            checkForm: {
                id: 2,
                name: '',
                gender: '',
                identifier: '',
                date: [],
                region: ''
            },
            checkLineForm: {
                id: '',
                name: '',
                gender: '',
                identifier: '',
                date: [],
                region: ''
            },
            look: {
                id: '',
                name: '',
                gender: '',
                identifier: '',
                date: [],
                region: ''
            },
            rules1:{
                date:[
                    { validator: dateTime,trigger: 'change'}
                ]
            },
            rules: {
                id: [
                    { required: true, message: '请输入工号', trigger: 'blur' },
                ],
                name: [
                    { required: true, message: '请输入姓名', trigger: 'change' },
                ],
                gender: [
                    { required: true, message: '请选择性别', trigger: 'change' }
                ],
                identifier: [
                    { required: true, message: '请输入身份证号', trigger: 'change' },
                ],
                region: [
                    { required: true, message: '请选择城市', trigger: 'blur' }
                ],
                date: [
                    { required: true, message: '请选择日期', trigger: 'blur' }
                ]
            },
            dialogFormVisible:false,
            dialogFormVisibleTwo:false,
            indexForm:null,
            tableData:[
                {
                    id: '1',
                    name: 'sdd',
                    gender: '男',
                    identifier: 'xxxxxxxxxxxxx',
                    date1: '2000-01-01',
                    date2: '2000-01-02',
                    region: '大连'
                },
                {
                    id: '1',
                    name: 'sdd',
                    gender: '男',
                    identifier: 'xxxxxxxxxxxxx',
                    date1: '2000-01-01',
                    date2: '2000-01-02',
                    region: '大连'
                }
            ],
            domains:[]
        }
    },
    methods: {
        //添加里的提交
        submitForm(formName) {
            this.$refs[formName].validate((valid) => {
                if (valid) {
                    // alert('submit!');
                    this.tableData.push({
                        id:this.form.id,
                        name:this.form.name,
                        date1:this.form.date[0],
                        date2:this.form.date[1],
                        gender:this.form.gender,
                        region:this.form.region,
                        identifier:this.form.identifier
                    });
                    this.$refs[formName].resetFields();
                    this.dialogFormVisible = false;
                } else {
                    // console.log('error submit!!');
                    return false;
                }
            });
            console.log('domains',this.domains);
        },
        //修改里的完成
        changeForm(formName){
            this.$refs[formName].validate((valid) => {
                if (valid) {
                    console.log('valid',valid)
                    console.log("checkForm",this.checkForm)
                    this.tableData[this.indexForm].id=this.checkForm.id;
                    this.tableData[this.indexForm].name=this.checkForm.name;
                    this.tableData[this.indexForm].date1=this.checkForm.date[0];
                    this.tableData[this.indexForm].date2=this.checkForm.date[1];
                    this.tableData[this.indexForm].gender=this.checkForm.gender;
                    this.tableData[this.indexForm].region=this.checkForm.region;
                    this.tableData[this.indexForm].identifier=this.checkForm.identifier;
                    this.dialogFormVisibleTwo = false;
            console.log("tableData",this.tableData[this.indexForm])
                } else {
                    return false;
                }
            });
        },
        //弹出添加dialog时，点击x和遮罩层要做的事
        handleClose(){
            this.$refs['form'].resetFields();
            this.dialogFormVisible = false;
        },
        //添加里的取消
        cc(formName){
            this.dialogFormVisible = false;
            this.$refs[formName].resetFields();
        },
        //重置
        resetForm(formName) {
            this.$refs[formName].resetFields();
        },
        //删除
        deleteRow(index, rows) {
            rows.splice(index, 1);
        },
        //显示修改前信息
        handleEdit(index, rows) {
            this.domains=this.tableData;
            console.log('domains',this.domains);
            console.log(index,rows)
            this.indexForm=index;
            // this.dialogFormVisibleTwo = true;
            this.checkLineForm.id=rows.id;
            this.checkLineForm.name=rows.name;
            this.checkLineForm.date[0]=rows.date1;
            this.checkLineForm.date[1]=rows.date2;
            this.checkLineForm.gender=rows.gender;
            this.checkLineForm.region=rows.region;
            this.checkLineForm.identifier=rows.identifier;
            console.log('checkForm',this.checkForm);
            this.checkForm.id=rows.id;
            this.checkForm.name=rows.name;
            this.checkForm.date[0]=rows.date1;
            this.checkForm.date[1]=rows.date2;
            this.checkForm.gender=rows.gender;
            this.checkForm.region=rows.region;
            this.checkForm.identifier=rows.identifier;
            rows.id='';
            rows.name='';
            rows.date1='';
            rows.date2='';
            rows.gender='';
            rows.region='';
            rows.identifier='';
        },
        //行内修改取消
        abolishChange(index, rows) {
            console.log(index,rows)
            this.indexForm=index;
            console.log('checkForm',this.checkForm);
            rows.id=this.checkLineForm.id;
            rows.name=this.checkLineForm.name;
            rows.date1=this.checkLineForm.date[0];
            rows.date2=this.checkLineForm.date[1];
            rows.gender=this.checkLineForm.gender;
            rows.region=this.checkLineForm.region;
            rows.identifier=this.checkLineForm.identifier;
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">

    .el-input{
        width: 150px;
        height: 26px;
    }
    .el-form-item {
        /*height: 26px;*/
        margin-left: 10px;
        margin-top: 20px;
    }

</style>
<style>
    .el-form-item__label{
        font-size: 16px;
    }
    .el-form-item.is-error .el-input__inner, .el-form-item.is-error .el-input__inner:focus, .el-form-item.is-error .el-textarea__inner, .el-form-item.is-error .el-textarea__inner:focus, .el-message-box__input input.invalid, .el-message-box__input input.invalid:focus{
        height: 30px;
    }
    .el-input__inner{
        height: 30px;
        line-height: 30px;
    }
    .el-date-editor--datetimerange{
        margin-top: 5px;
    }

</style>