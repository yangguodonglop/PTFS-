<template>
	<section>
		<el-tabs v-model="activeName" @tab-click="handleClick">
    	<el-tab-pane label="文件查询" name="first">
			<!--工具条-->
		<el-col :span="24" class="toolbar" style="padding-bottom: 0px;">
			<el-form :inline="true" ref="findForm" :model="findForm">
				<el-form-item label="开始时间">
					 <el-date-picker
						v-model="findForm.startTime"
						type="datetime"
						@change="getSTimestart"
						 format="yyyy-MM-dd HH:mm"
						placeholder="选择日期时间">
   					 </el-date-picker>
				</el-form-item>
				<el-form-item label="结束时间">
					 <el-date-picker
						v-model="findForm.endTime"
            	@change="getSTimeend"
						type="datetime"
            format="yyyy-MM-dd HH:mm"
						placeholder="选择日期时间">
   					 </el-date-picker>
				</el-form-item>
				<el-form-item label="用户ID">
					<el-input v-model="findForm.userId"></el-input>
				</el-form-item>
				<el-form-item label="文件名">
					<el-input v-model="findForm.fileName"></el-input>
				</el-form-item>
				<el-form-item label="矿机节点ID">
					<el-input v-model="findForm.minerId" style="width:400px"></el-input>
				</el-form-item>
         <el-form-item label="文件哈希" >
					<el-input v-model="findForm.fileHashId" style="width:468px"></el-input>
				</el-form-item>
				<el-form-item>
					<el-button type="primary"  @click="onSubmit">查询</el-button>
				</el-form-item>
			</el-form>
		</el-col>

		<!--列表-->
		<template>
			<el-table :data="tableData.slice((currentPageCx-1)*pagesizeCx,currentPageCx*pagesizeCx)" highlight-current-row v-loading="listLoading" style="width: 100%;">
			<!-- <el-table-column type="selection" width="55">
			</el-table-column> -->
			<el-table-column type="index" width="60">
			</el-table-column>
			<el-table-column prop="fileName" label="文件名" width="200" sortable>
			</el-table-column>
			<el-table-column prop="fileSize" label="文件大小" width="150"  sortable>
			</el-table-column>
			<el-table-column prop="fileHashId" label="文件哈希" width="500" sortable>
			</el-table-column>
			<el-table-column prop="fileStoreTime" label="上传详情"  sortable>
			</el-table-column>
			<el-table-column label="操作" width="150">
				<template scope="scope">
					<el-button size="small" @click="handleEdit(scope.$index, scope.row)">查看详情</el-button>
					<!-- <el-button type="danger" size="small" @click="handleDel(scope.$index, scope.row)">删除</el-button> -->
				</template>
			</el-table-column>
		</el-table>
		<!--工具条-->
		<el-col :span="24" class="toolbar">
			 <el-pagination
                            @size-change="handleSizeChangeCx"
                            @current-change="handleCurrentChangeCx"
                            :current-page="currentPageCx"
                            :page-sizes="[5, 10, 20]" 
                            :page-size="pagesizeCx"        
                            layout="total, sizes, prev, pager, next, jumper"
                            :total="tableData.length">    
                    </el-pagination>
		</el-col>
		</template>
	</el-tab-pane>
    <el-tab-pane label="黑名单" name="second">
			<!--工具条-->
		<el-col :span="24" class="toolbar" style="padding-bottom: 0px;">
				<el-form :inline="true" ref="blackListForm" :model="blackListForm">
				<el-form-item label="开始时间">
					 <el-date-picker
						v-model="blackListForm.startTime"
						type="datetime"	
						@change="getSTimestartBlackList"
						 format="yyyy-MM-dd HH:mm"
						placeholder="选择日期时间">
   					 </el-date-picker>
				</el-form-item>
				<el-form-item label="结束时间">
					 <el-date-picker
						v-model="blackListForm.endTime"
						type="datetime"
           @change="getSTimeendBlackList"
           format="yyyy-MM-dd HH:mm"
						placeholder="选择日期时间">
   					 </el-date-picker>
				</el-form-item>
				<el-form-item label="文件名">
					<el-input v-model="blackListForm.fileName"></el-input>
				</el-form-item>
				<el-form-item label="文件哈希">
					<el-input v-model="blackListForm.fileHashId" style="width:450px;"></el-input>
				</el-form-item>
				<el-form-item label="描述">
					<el-input v-model="blackListForm.descript"></el-input>
				</el-form-item>
				<el-form-item>
					<el-button type="primary"  @click="onSubmitBlackList('single')">查询</el-button>
					<el-button type="primary"  @click="onSubmitBlackList('all')">加载所有黑名单</el-button>
					<el-button type="primary"  @click="onCreate('single')">新增</el-button>
					<el-button type="primary"  @click="onCreate('all')">批量新增</el-button>
				</el-form-item>
			</el-form>
		</el-col>

		<!--列表-->
		<template>
			<el-table :data="tableDataBlacklist.slice((currentPageHmd-1)*pagesizeHmd,currentPageHmd*pagesizeHmd)" highlight-current-row v-loading="loading" style="width: 100%;">
				<el-table-column type="index" width="60">
				</el-table-column>
				<el-table-column prop="fileName" label="文件名" sortable>
				</el-table-column>
				<el-table-column prop="fileHashId" label="文件哈希"  sortable>
				</el-table-column>
				<el-table-column prop="version" label="版本"  sortable>
				</el-table-column>
				<el-table-column prop="descript" label="描述" sortable>
				</el-table-column>
	
        <el-table-column label="操作" width="150">
				<template scope="scope">
					<el-button type="danger" size="small" @click="handleDel(scope.$index, scope.row)">删除</el-button>
				</template>
			</el-table-column>
				
			</el-table>
      <el-pagination
        @size-change="handleSizeChangeHmd"
                            @current-change="handleCurrentChangeHmd"
                            :current-page="currentPageHmd"
                            :page-sizes="[5, 10, 20]" 
                            :page-size="pagesizeHmd"        
                            layout="total, sizes, prev, pager, next, jumper"
                            style="float:right;margin-top:10px;"
                            :total="tableDataBlacklist.length">
			</el-pagination>
		</template>
	</el-tab-pane>

  </el-tabs>
  <el-dialog title="" :visible.sync="dialogTableVisible"   customClass="customWidth">
	  <div class="dialog_item_title">
		  <el-col :span="9">
			 <div class="item_title_text" style="width:100%">
				 	文件哈希：{{titleData.fileHashId}}
			 </div>
		  </el-col>
		    <el-col :span="3">
			 <div class="item_title_text">
				 	大小:{{titleData.fileSize}}
			 </div>
		  </el-col>
		    <el-col :span="5">
			  <div class="item_title_text">
				 	上传时间： {{titleData.fileStoreTime}}
			 </div>
		  </el-col>
		    <el-col :span="4">
			  <div class="item_title_text">
				 	总下载量： {{titleData.fileName}}
			 </div>
		  </el-col>
		   <el-col :span="3">
			  <div class="item_title_text">
				<el-button type="primary" @click="backUp()">备份</el-button>
				<el-button type="danger"   @click="deleteData()">删除</el-button>
			 </div>
		  </el-col>
	  </div>
	  <div class="dialog_item_table">
      <el-tabs v-model="activeNameXq" @tab-click="handleClick">
        <el-tab-pane label="备份数量" name="first">
            <el-col :span="24">
              <div class="item_title_h1">
                备份数量
              </div>
              <div class="item_title_child">
                  <el-table :data="tableDataBf.slice((currentPageBf-1)*pagesizeBf,currentPageBf*pagesizeBf)">
                    <el-table-column prop="storeNodeId" label="节点哈希" align="center" ></el-table-column>
                    <el-table-column property="storeTimestamp" label="上传时间" align="center" ></el-table-column>
                    <el-table-column property="nodeStoreUsage" label="节点空间" align="center"></el-table-column>
                  </el-table>
                  <el-pagination
                     @size-change="handleSizeChangeBf"
                            @current-change="handleCurrentChangeBf"
                            :current-page="currentPageBf"
                            :page-sizes="[5, 10, 20]" 
                            :page-size="pagesizeBf"        
                            layout="total, sizes, prev, pager, next, jumper"
                            style="float:right"
                            :total="tableDataBf.length"> 
                  </el-pagination>
              </div>
		       </el-col>
        </el-tab-pane>
        <el-tab-pane label="文件拥有者" name="second">
           <el-col :span="24">
              <div class="item_title_h1">
                文件拥有者
              </div>
              <div class="item_title_child">
                  <el-table :data="tableDataYyz.slice((currentPageYyz-1)*pagesizeYyz,currentPageYyz*pagesizeYyz)">
                    <el-table-column prop="owner" label="用户名" align="center" ></el-table-column>
                    <el-table-column property="fileName" label="文件名" align="center" ></el-table-column>
                    <el-table-column property="timestamp" label="上传时间" align="center"></el-table-column>
                  </el-table>
                  <el-pagination
                    @size-change="handleSizeChangeYyz"
                            @current-change="handleCurrentChangeYyz"
                            :current-page="currentPageYyz"
                            :page-sizes="[5, 10, 20]" 
                            :page-size="pagesizeYyz"        
                            layout="total, sizes, prev, pager, next, jumper"
                            style="float:right"
                            :total="tableDataYyz.length">   
                  </el-pagination>
              </div>
            </el-col>

        </el-tab-pane>
        <el-tab-pane label="日志记录" name="third">
             <el-col :span="24">
              <div class="item_title_h1">
                日志记录
              </div>
              <div class="item_title_child">
                  <el-table :data="tableDataRzjl.slice((currentPageRz-1)*pagesizeRz,currentPageRz*pagesizeRz)">
                    <el-table-column prop="timestamp" label="时间" align="center" ></el-table-column>
                    <el-table-column property="action" label="Action" align="center"  ></el-table-column>
                    <el-table-column property="code" label="Code" align="center"></el-table-column>
                    <el-table-column property="descript" label="描述" align="center"></el-table-column>
                  </el-table>
                  <el-pagination
                   @size-change="handleSizeChangeRz"
                            @current-change="handleCurrentChangeRz"
                            :current-page="currentPageRz"
                            :page-sizes="[5, 10, 20]" 
                            :page-size="pagesizeRz"        
                            layout="total, sizes, prev, pager, next, jumper"
                            style="float:right"
                            :total="tableDataRzjl.length">    
                  </el-pagination>
              </div>
		       </el-col>
        </el-tab-pane>
        
    </el-tabs>

		   
		  
		  
	  </div>
	
</el-dialog>
 <el-dialog title="" :visible.sync="dialogCreateVisible"   customClass="customWidth1">
	  <div class="dialog_item_title">
		  <el-col :span="24">
			 <div class="item_title_create">
				 	<el-form :inline="true" ref="createForm" :model="createForm">
            <el-col :span="24">
              <div style="width:80%;margin:0 auto;">
              <el-col :span="4">
              <div class="create_div">文件名:</div>
              </el-col>
                <el-col :span="20">
              <div class="create_div_input">
                <el-input v-model="createForm.fileName" placeholder="请输入文件名"></el-input>
              </div>
              </el-col>
              </div>
                
           </el-col>
            <el-col :span="24">
               <div style="width:80%;margin:0 auto;">
              <el-col :span="4">
              <div class="create_div">文件ID:</div>
              </el-col>
                <el-col :span="20">
              <div class="create_div_input">
                <el-input v-model="createForm.fileHashId" placeholder="请输入文件ID"></el-input>
              </div>
              </el-col>
                  </div>
                
           </el-col>
            <el-col :span="24">
               <div style="width:80%;margin:0 auto;">
              <el-col :span="4">
              <div class="create_div">文件描述:</div>
              </el-col>
                <el-col :span="20">
              <div class="create_div_input">
                <el-input v-model="createForm.descript" placeholder="请输入文件描述"></el-input>
              </div>
              </el-col>
                    </div>
           </el-col>
           
        <el-col :span="24">
          <div style="width:60px; margin:20px auto;">
				<el-form-item>
					<el-button type="primary" @click="onSubmitAdd()"  >确定</el-button>
				</el-form-item>
        </div>
          </el-col>
          
			</el-form>
      
			 </div>
		 </el-col>
	  </div>
</el-dialog>

<el-dialog title="" :visible.sync="dialogAllCreateVisible"   customClass="customWidth1">
	  <div class="dialog_item_title">
		  <el-col :span="24">
			 <div class="item_title_create">
				 	<el-form :inline="true" ref="createForm" :model="createForm">
             <el-col :span="24">
              <div style="width:80%;margin:0 auto;">
              <el-col :span="4">
              <div class="create_div">文件名:</div>
              </el-col>
                <el-col :span="20">
              <div class="create_div_input">
                  <el-upload
                   class="upload-demo"
                    ref="upload"
                    drag
                    name="file"
                    :before-upload="beforeAvatarUpload" 
                    action=""
                    :file-list="fileList"
                    :http-request="uploadSectionFile"
                    :auto-upload="false">
                    <i class="el-icon-upload"></i>
                    <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
                    <div class="el-upload__tip" slot="tip">只能上传.bl文件.</div>
                  </el-upload>
              </div>
              </el-col>
              </div>   
           </el-col>
        <el-col :span="24">
          <div style="width:60px; margin:20px auto;">
				<el-form-item>
					<el-button type="primary" @click="submitUpload()"  >确定</el-button>
				</el-form-item>
        </div>
          </el-col>
          
			</el-form>
      
			 </div>
		 </el-col>
	  </div>
</el-dialog>
	

	</section>
</template>
<script>
import { getUserList } from "../../api/api";
//import NProgress from 'nprogress'
import {
  queryFileSummaryByConditions,
  queryOneFileBackupOwnerRecords,
  queryBlackListByCondition,
  deleteFileList,
  addNewForbiddenFile,
  backupFileByCondition,
  uploadBlackListFile,
  updateBlackListFile
} from "../../api/api";
export default {
  data() {
    return {
      userId: "blackListFile",
      fileList: [],
      loading: false,
      tableDataBlacklist: [],
      titleData: {},
      tableDataYyz: [],
      tableDataBf: [],
      tableDataRzjl: [],
      findForm: {
        startTime: "",
        endTime: "",
        userId: "",
        fileHashId: "",
        fileName: "",
        minerId: ""
      },
      blackListForm: {
        startTime: "",
        endTime: "",
        fileName: "",
        fileHashId: "",
        descript: ""
      },
      createForm: {
        descript: "",
        fileHashId: "",
        fileName: ""
      },
      form: {
        name: "",
        region: "",
        date1: "",
        date2: "",
        delivery: false,
        type: [],
        resource: "",
        desc: "",
        startTime: "2018-01-01",
        endTime: "2019-01-01",
        userId: "",
        fileHashId: "",
        fileName: "",
        minerId: ""
      },

      tableData: [],
      currentPage: 1,
      pagesize: 10,
      currentPageCx: 1,
      pagesizeCx: 10,
      currentPageHmd: 1,
      pagesizeHmd: 10,
      currentPageBf: 1,
      pagesizeBf: 10,
      currentPageRz: 1,
      pagesizeRz: 10,
      currentPageYyz: 1,
      pagesizeYyz: 10,

      dialogTableVisible: false,
      dialogCreateVisible: false,
      dialogAllCreateVisible: false,
      filters: {
        name: ""
      },
      activeNameXq: "first",
      activeName: "first",
      users: [],
      total: 0,
      page: 1,
      listLoading: false,
      sels: [] //列表选中列
    };
  },
  mounted() {
    //获取当前时间
    let nowDate = new Date();
    let endTime = this.common.getTimes(nowDate);
    this.findForm.endTime = endTime; // 当前的时间点
    this.blackListForm.endTime = endTime;

    //获取往前往前一星期时间
    let befDate = new Date(nowDate.getTime() - 7 * 24 * 3600 * 1000);
    let startTime = this.common.getTimes(befDate);
    this.findForm.startTime = startTime; // 向前推迟一周的时间点
    this.blackListForm.startTime = startTime;

    this.getUsers();
    this.getBlacklist();
  },
  methods: {
    submitUpload() {
      this.dialogAllCreateVisible = false;
      let list = document.getElementsByClassName(
        "el-upload-list__item is-ready"
      );
      if (list.length == 0) {
        this.$message({
          type: "warning",
          message: "请选择需要导入的模板！"
        });
        return;
      }
      this.$refs.upload.submit();
    },
    uploadSectionFile(param) {
      var fileObj = param.file;
      // FormData 对象
      var form = new FormData();
      // 文件对象
      form.append("file", fileObj);
      uploadBlackListFile(form).then(res => {
        if (res.result == "ok") {
          //  this.$message({
          //    type:'success',
          //    message:res.msg
          //  })
          this.getNewBlacklist()    
        } else {
        }
      });
    },

    onSubmitAddall: function() {
      this.dialogAllCreateVisible = false;
    },
    beforeAvatarUpload(file) {
      var fileName = new Array();
      fileName = file.name.split(".");
      const extension = fileName[fileName.length - 1] === "bl";
      const isLt2M = file.size / 1024 / 1024 < 10;
      if (!extension) {
        this.$message({
          message: "上传模板只能是.bl格式!",
          type: "warning"
        });
        //
      }
      //                 if (!isLt2M) {
      //                     this.$message({
      //                         message: '上传模板大小不能超过 10MB!',
      //                         type: 'warning'
      //                     });
      // //                    console.log('上传模板大小不能超过 10MB!')
      //                 }
      // if (extension || extension2 && isLt2M == true) {
      //     console.log(file)
      //     let fd = new FormData()
      //     fd.append('invoiceTypeId', this.invoice_type_id)//随文件上传的其他参数
      //     fd.append('epid', this.epid)
      //     fd.append('file', file)
      //     // console.log(fd)
      //     this.newImport(fd).then(function (res) {//校验完成后提交
      //         console.log(res)
      //     }, function () {
      //         console.log('failed');
      //     });
      //     return true
      // }
      return extension;
    },

    //黑名单新增
    onSubmitAdd: function() {
      let param = this.createForm;
      this.dialogCreateVisible = false;
      addNewForbiddenFile(param).then(res => {
        let { result, msg, data } = res;
        if (result == "ok") {
            this.getNewBlacklist()    
        } else {
          this.$message({
            message: msg,
            type: "error"
          });
        }
      });
    },
    //查看详情备份
    backUp: function() {
      let param = new Object();
      param.fileHashId = this.titleData.fileHashId;
      param.backUpCount = 1;
      backupFileByCondition(param).then(res => {
        let { result, msg, data } = res;
        if (result == "ok") {
          this.$message({
            message: msg,
            type: "success"
          });
        } else {
          this.$message({
            message: msg,
            type: "error"
          });
        }
      });
    },

    //查看详情删除
    deleteData: function() {
      let param = new Object();
      param.FileHashId = this.titleData.fileHashId;
      deleteFileList(param).then(res => {
        let { result, msg, data } = res;
        if (result == "ok") {
          this.$message({
            message: msg,
            type: "success"
          });
        } else {
          this.$message({
            message: msg,
            type: "error"
          });
        }
      });
    },
    //分页
    handleCurrentChangeCx: function(currentPage) {
      this.currentPageCx = currentPage; //点击第几页
    },
    //改变下拉分页
    handleSizeChangeCx: function(size) {
      this.pagesizeCx = size; //每页下拉显示数据
    },
    handleCurrentChangeHmd: function(currentPage) {
      this.currentPageHmd = currentPage; //点击第几页
    },
    //改变下拉分页
    handleSizeChangeHmd: function(size) {
      this.pagesizeHmd = size; //每页下拉显示数据
    },
    handleCurrentChangeBf: function(currentPage) {
      this.currentPageBf = currentPage; //点击第几页
    },
    //改变下拉分页
    handleSizeChangeBf: function(size) {
      this.pagesizeBf = size; //每页下拉显示数据
    },
    handleCurrentChangeYyz: function(currentPage) {
      this.currentPageYyz = currentPage; //点击第几页
    },
    //改变下拉分页
    handleSizeChangeYyz: function(size) {
      this.pagesizeYyz = size; //每页下拉显示数据
    },
    handleCurrentChangeRz: function(currentPage) {
      this.currentPageRz = currentPage; //点击第几页
    },
    //改变下拉分页
    handleSizeChangeRz: function(size) {
      this.pagesizeRz = size; //每页下拉显示数据
    },
    //黑名单新增
    onCreate: function(e) {
      if (e === "all") {
        this.dialogAllCreateVisible = true;
      } else {
        this.dialogCreateVisible = true;
      }
    },
    //黑名单批量添加
    onAllCreate: function() {},
    //详情选项卡
    handleClick(tab, event) {
      console.log(tab, event);
    },

    getSTimestart(val) {
      this.findForm.startTime = val; //获取文件查询开始时间改变是的值
    },
    getSTimeend(val) {
      this.findForm.endTime = val; //获取文件查询结束时间改变是的值
    },
    getSTimestartBlackList(val) {
      this.blackListForm.startTime = val; //获取黑名单开始时间改变是的值
    },
    getSTimeendBlackList(val) {
      this.blackListForm.endTime = val; //获取黑名单结束时间改变是的值
    },

    //获取用户列表
    getUsers() {
      let param = this.findForm;
      this.listLoading = true;
      //NProgress.start();
      queryFileSummaryByConditions(param).then(res => {
        let { result, msg, data } = res;
        if ((result = "ok")) {
          this.listLoading = false;
          this.tableData = res.data;
        }
      });
    },

    //获取黑名单列表
    getBlacklist() {
      let param = this.blackListForm;
      this.listLoading = true;
      //NProgress.start();
      queryBlackListByCondition(param).then(res => {
        let { result, msg, data } = res;
        if ((result = "ok")) {
          this.listLoading = false;
          this.tableDataBlacklist = res.data;
        
        }
      });
    },

    //新增，批量新增，删除后不传结束时间的查询
  getNewBlacklist(){
    let newparam = {};
          newparam.startTime = this.blackListForm.startTime;
          newparam.descript = "";
          newparam.fileHashId = "";
          newparam.fileName = "";
          newparam.endTime = "";

          this.listLoading = true;
          //NProgress.start();

          queryBlackListByCondition(newparam).then(res => {
            let { result, msg, data } = res;
            if ((result = "ok")) {
              this.listLoading = false;
              this.tableDataBlacklist = res.data;
              console.log(res.data);
            }
          });
  },
    //删除
    handleDel: function(index, row) {
      this.$confirm("确认删除该记录吗?", "提示", {
        type: "warning"
      })
        .then(() => {
          let param = {
            FileHashId: row.fileHashId
          };
          updateBlackListFile(param).then(res => {
            let { result, msg, data } = res;
            if ((result = "ok")) {
              this.getNewBlacklist()
            }
          });
        })
        .catch(() => {
          this.$message({
            message: msg,
            type: "error"
          });
        });
    },
    //查看详情页面
    handleEdit: function(index, row) {
      this.activeNameXq = "first";
      this.titleData = row;
      let param = new Object();
      param.FileHashId = row.fileHashId;
      queryOneFileBackupOwnerRecords(param).then(res => {
        let { result, msg, data } = res;
        if (result == "ok") {
          this.dialogTableVisible = true;
          this.tableDataBf = res.data.fileBackUpList;
          this.tableDataYyz = res.data.fileOwnerList;
          this.tableDataRzjl = res.data.fileExpLogList;
          console.log(this.tableDataBf);
        } else {
          this.$message({
            message: msg,
            type: "error"
          });
        }
      });

    },

    handleClick(tab, event) {
      console.log(tab, event);
    },
    getUser: function() {},

    //文件查询
    onSubmit() {
      let param = this.findForm;
      this.listLoading = true;
      //NProgress.start();
      console.log(param);
      queryFileSummaryByConditions(param).then(res => {
        let { result, msg, data } = res;
        if (result == "ok") {
          this.listLoading = false;
          this.tableData = res.data;
        } else {
          this.$message({
            message: "msg",
            type: "error"
          });
        }
      });
    },
    //黑名单查询
    onSubmitBlackList(e) {
      let param = "";
      if (e === "all") {
        this.blackListFormAll = {
          startTime: "",
          endTime: "",
          fileName: "",
          fileHashId: "",
          descript: ""
        };
        param = this.blackListFormAll;
      } else {
        param = this.blackListForm;
      }

      this.dialogCreateVisible = false;
      queryBlackListByCondition(param).then(res => {
        let { result, msg, data } = res;
        if (result == "ok") {
          this.listLoading = false;
          this.tableDataBlacklist = res.data;
        } else {
          this.$message({
            message: "msg",
            type: "error"
          });
        }
      });
    }
  }
};
</script>

<style lang="less">
.customWidth {
  width: 85%;
}
.customWidth1 {
  width: 30%;
}
.dialog_item_title {
  width: 100%;
  overflow: hidden;
  height: auto;
}
.item_title_text {
  width: 80%;
  margin: 0 auto;
  height: 50px;
  background: #f2f2f2;
  line-height: 50px;
  text-align: center;
}
.item_title_h1 {
  width: 90%;
  line-height: 50px;
  margin: 0 auto;
}
.dialog_item_table {
  width: 100%;
  height: auto;
   
  padding: 30px auto;
  overflow: hidden;
  margin: 50px auto;
  .item_title_child {
    width: 90%;
    margin: 0 auto;
    height: auto;
    background: #f2f2f2;
    overflow: hidden;
    text-align: center;
  }
}
.item_title_create {
  height: auto;
  overflow: hidden;
}
.create_div {
  width: 150px;
  line-height: 38px;
  text-align: left;
  margin-top: 15px;
}
.create_div_input {
  float: left;
  margin-top: 15px;
  .el-input__inner {
    width: 300px;
  }
}
</style>
