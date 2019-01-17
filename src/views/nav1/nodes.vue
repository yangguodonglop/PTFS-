<template>
	<section class="chart-container">
		<!--工具条-->
	
		<el-col :span="24" class="toolbar" style="padding-bottom: 0px;">
				<el-row>
					<el-col :span="24">
						<div class="grid-content bg-purple new_grid-content">
							<el-tabs v-model="activeName" @tab-click="handleClick">
								<el-tab-pane label="节点分布" name="first">
									<el-col :span="14">
                    <div class="div_MapDetails" v-if="MapDetailsShow">
            
                      <p  style=" width:200px;word-wrap:break-word;margin-top:20px;white-space: nowrap; overflow: hidden;text-overflow: ellipsis;">节点ID:{{details_minerId}}</p>
                      <p>节点类型:{{details_lx}}</p>
                      <p>节点所在地：{{details_city}}-{{details_region}}</p>
                      <p style=" width:200px;word-wrap:break-word;margin-top:0px;white-space: nowrap; overflow: hidden;text-overflow: ellipsis;">节点IP：{{details_Ip}}</p>
                      <p>节点状态：{{details_nodeState}}</p>
                       <p style="padding-bottom:30px">	<el-button type="primary" @click="getMoreDetails()">更多详情</el-button></p>

                    </div>
									<div class="item_one_child" id="chartColumn">
										<ve-map :data="chartData" width="800px" height="600px" ></ve-map>
									</div>
                  <!-- <div id="chartColumn" style="width: 100%;height:800px;"></div> -->
									</el-col>
									<el-col :span="10">
									<div class="item_one_child_r">
										<div class="child_r_t">
												<el-checkbox-group v-model="checkList">
                          <el-checkbox v-for="item in testarray"  :key="item.id" :label="item.name" :value="item.id"  @change="handleCheckedChange(item)">{{item.name}}</el-checkbox>
											
											</el-checkbox-group>
										</div>
                    <div class="child_r_t"  style="margin-top:20px;">
                    		<el-button type="primary" @click="getNodesNewDetails()">刷新</el-button>
                    </div>
										<div class="child_r_b" v-for="item in nodeTypeCount" :key="item.id">
											{{item.nodeType}}：	{{item.nodeCount}}
										</div>
										<div class="child_r_table">
											<el-table
												:data="tableDataDetails.slice((currentPageJd-1)*pagesizeJd,currentPageJd*pagesizeJd)"
                        @row-click="openDetails"
                        >
												<el-table-column
													prop="nodeType"
													label="类型"
                          width="120"
												>
												</el-table-column>
												<el-table-column
													prop="nodeId"
													label="节点ID"
                          
												>
												</el-table-column>
											</el-table>
											  <el-pagination
                   @size-change="handleSizeChangeJd"
                            @current-change="handleCurrentChangeJd"
                            :current-page="currentPageJd"
                            :page-sizes="[5, 10, 20]" 
                            :page-size="pagesizeJd"        
                            layout="total, sizes, prev, pager, next, jumper"
                            style="float:right"
                            :total="tableDataDetails.length">    
                  </el-pagination>
										</div>
									</div>
									</el-col>
								</el-tab-pane>
								<el-tab-pane label="节点查询" name="second">
									<div class="item_two_child">
										<div class="item_top">
											<div class="item_top_id">节点ID</div>
											<div class="item_top_input">
												<el-input v-model.trim="input_nodeId" placeholder="请输入节点ID查询"></el-input>
											</div>
											<div class="item_top_button">
												<el-button type="primary" @click="getNodesDetails()">查询</el-button>
											</div>
										</div>
										<div class="item-bottom">
												 <p>节点ID:<span style=" width:200px;word-wrap:break-word;margin-top:20px;white-space: nowrap; overflow: hidden;text-overflow: ellipsis;">{{details_minerId}}</span></p>
                      <p>节点类型:{{details_lx}}</p>
                      <p>节点所在地：{{details_city}}-{{details_region}}</p>
                      <p >节点IP：<span style=" width:200px;word-wrap:break-word;margin-top:0px;white-space: nowrap; overflow: hidden;text-overflow: ellipsis;">{{details_Ip}}</span></p>
                      <p>节点状态：{{details_nodeState}}</p>
                       <p style="padding-bottom:30px">	<el-button type="primary" @click="getMoreDetails()">更多详情</el-button></p>
										</div>
									</div>
								</el-tab-pane>
								<el-tab-pane label="更新节点配置" name="fourth">
                 
									<div class="item_three_child">
										<div class="three_child_button">
											<el-button type="primary" @click="createFrom">新增</el-button>
											<el-button type="primary" @click="getNodeslist()">刷新</el-button>
										</div>
									</div>
                  <div class="item_three_table">
                      <el-table
												:data="tableDataNodeConfig.slice((currentPagePz-1)*pagesizePz,currentPagePz*pagesizePz)"
												style="width: 100%">
                        <el-table-column
                         type="index">
                         </el-table-column>
												<el-table-column
													prop="timeStamp"
													label="时间">
												</el-table-column>
												<el-table-column
													prop="msgId"
													label="命令编号">
												</el-table-column>
												<el-table-column
													prop="commandTargetId"
													label="目标节点">
												</el-table-column>
                        <el-table-column
													prop="commandExecResult"
													label="执行结果">
												</el-table-column>
                        <el-table-column
													prop="commandExecData"
													label="命令附件参数">
                          
												</el-table-column>
                     
                      
                        <el-table-column
													prop="descript"
													label="描述">
                         
												</el-table-column>
                           <el-table-column
													label="操作">
                            <template slot-scope="scope">
                                <el-button  type="text" size="small" @click.native="downLoadFile(scope.row)">下载</el-button>
                               
                            </template>
                          
												</el-table-column>
											</el-table>
											  <el-pagination
                   @size-change="handleSizeChangePz"
                            @current-change="handleCurrentChangePz"
                            :current-page="currentPagePz"
                            :page-sizes="[5, 10, 20]" 
                            :page-size="pagesizePz"        
                            layout="total, sizes, prev, pager, next, jumper"
                            style="float:right"
                            :total="tableDataNodeConfig.length">    
                  </el-pagination>
                  </div>
                  
								</el-tab-pane>
							</el-tabs>
						</div>
					</el-col>
					<!-- <el-col :span="6"><div class="grid-content bg-purple-light">2</div></el-col> -->
				</el-row>
		</el-col>
  <el-dialog title="" :visible.sync="dialogTableVisible"  id="el-dialog-new" customClass="customWidth">
	 
	  <div class="dialog_item_table">
		   <el-col :span="24">
			<div class="item_title_h1">
				节点存储文件列表
			</div>
			 <div class="item_title_child">
				 	<el-table :data="tableDataMoreDetails.slice((currentPageCc-1)*pagesizeCc,currentPageCc*pagesizeCc)">
             <el-table-column type="index">
				</el-table-column>
						<el-table-column prop="fileName" label="文件名" ></el-table-column>
						<el-table-column property="fileSize" label="文件大小" ></el-table-column>
						<el-table-column property="fileCreateTime" label="上传时间"></el-table-column>
            <el-table-column property="fileHashId" label="文件哈希"></el-table-column>
					</el-table>
					<el-pagination
						@size-change="handleSizeChangeCc"
                            @current-change="handleCurrentChangeCc"
                            :current-page="currentPageCc"
                            :page-sizes="[5, 10, 20]" 
                            :page-size="pagesizeCc"        
                            layout="total, sizes, prev, pager, next, jumper"
                            style="float:right"
                            :total="tableDataMoreDetails.length">  
					</el-pagination>
			 </div>
		  </el-col>

		
	  </div>
	
</el-dialog>

 <el-dialog title="" :visible.sync="dialogCreateVisible"   customClass="customWidth1">
	   <div class="dialog_item_title">
		  <el-col :span="24">
			 <div class="item_title_create">
				 	<el-form :inline="true" ref="form" :model="form">
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
                    name="configFile"
                    :before-upload="beforeAvatarUpload" 
                    action=""
                    
                    :file-list="fileList"
                    :http-request="uploadSectionFile"
                    :auto-upload="false">
                    <i class="el-icon-upload"></i>
                    <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
                    <div class="el-upload__tip" slot="tip">只能上传.cfg文件.</div>
                  </el-upload>
              </div>
              </el-col>
              </div>
                
           </el-col>
            <el-col :span="24">
              <div style="width:80%;margin:0 auto;">
              <el-col :span="4">
              <div class="create_div">描述:</div>
              </el-col>
                <el-col :span="20">
              <div class="create_div_input">
                <el-input v-model.trim="descript" placeholder="请输入文件名"></el-input>
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
//import util from "../../common/js/util";

import newUrl from "../../api/api";

import echarts from "echarts";
//import NProgress from 'nprogress'
import {
  queryMinerGeographDist,
  queryOneCityMinerDistDetails,
  queryNodeStoredFileListByNodeId,
  queryNodeInfoById,
  queryAllConfigRecords,
  updateNodeConfig,
  uploadBlackListFile,
  queryAllNodeProfiesByNodeTypes
} from "../../api/api";

export default {
  data() {
    return {
      newUrl: newUrl.customData(),//获取服务器 或则本地请求base
      nodeTypeCount: [],
      descript: "6",
      fileList: [],
      currentPageJd: 1, //初始页
      pagesizeJd: 10, //    每页的数据
      currentPagePz: 1, //初始页
      pagesizePz: 10, //    每页的数据
      currentPageCc: 1, //初始页
      pagesizeCc: 10, //    每页的数据
      tableData1: [],
      tableData: [],
      tableDataNodeConfig: [],
      chartEvents: "",
      testarray: [
        {
          id: "1",
          name: "超级管理节点"
        },
        {
          id: "2",
          name: "超级存储节点"
        },
        {
          id: "3",
          name: "矿机节点"
        },
        {
          id: "4",
          name: "应用层节点"
        }
      ],
      dialogTableVisible: false,
      dialogCreateVisible: false,
      activeName: "first",
      MapDetailsShow: false,
      chartColumn: null,
      details_gl: "",
      details_No: "",
      details_lx: "",
      details_time: "",
      details_minerId: "",
      details_length: "",
      details_Ip: "",
      details_nodeState: "",
      details_city: "",
      details_region: "",
      input_nodeId: "",
      downLoadfileName: "",

      form: {
        name: "",
        region: "",
        date1: "",
        date2: "",
        delivery: false,
        type: [],
        resource: "",
        desc: "",
        newvalue1: "",
        newvalue2: "",
        newvalue3: "",
        newvalue4: "",
        newvalue5: ""
      },

      //   activeNames: ["1"],
      input: "请输入",
      chartData: {
        columns: [],
        rows: [
          // { 位置: "吉林", 超级管理员: 55555,矿工节点:6666666, 超级储存节点: 44444,应用层节点:888888 },
          // { 位置: "北京", 超级管理员: 555551, 超级储存节点: 444441,应用层节点:8888881,矿工节点:66666661}
        ]
      },
      checkList: ["位置", "超级管理节点"],
      tableDataDetails: [],
      tableDataMoreDetails: []
    };
  },
  mounted() {
    // 获取节点分布列表
    this.getDetails();
    //获取节点配置列表
    this.getNodeslist();
    this.chartData.columns = this.checkList;
  },
  methods: {
    getNodesNewDetails() {
      // alert(JSON.stringify(this.chartData.rows));
      // this.chartData.rows = [{ 位置: "湖北", 税收: 888, 人口: 888, 面积: 666 }];
      this.getDetails();
    },
    //下载
    downLoadFile(row) {
      this.downLoadfileName = row.commandExecData;
      location.href =
        `${this.newUrl}/PTFSFileManage/downloadConfigFile?fileName=` +
        this.downLoadfileName;
    },

    //刷新节点配置更新列表
    getNodeslist() {
      this.getNodeslist();
    },
    //文件上传
    submitUpload() {
      this.dialogCreateVisible = false;
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
      form.append("setConfigDescript", this.descript);
      updateNodeConfig(form).then(res => {
        if (res.result == "success") {
          this.$message({
            type: "success",
            message: res.msg
          });
          this.getNodeslist();
        } else {
        }
      });
    },
    //c上传前验证
    beforeAvatarUpload(file) {
      var fileName = new Array();
      fileName = file.name.split(".");
      const extension = fileName[fileName.length - 1] === "cfg";
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

    //节点查询
    getNodesDetails() {
      this.MapDetailsShow = true;
      let param = {
        NodeId: this.input_nodeId
      };
      this.listLoading = true;
      queryNodeInfoById(param).then(res => {
        let { result, msg, data } = res;
        if ((result = "ok")) {
          this.listLoading = false;
          this.details_lx = res.data.nodeType;
          this.details_minerId = res.data.minerId;
          this.details_Ip = res.data.ip;
          this.details_nodeState = res.data.nodeState;
          this.details_city = res.data.city;
          this.details_region = res.data.region;
        }
      });
    },

    //获取更多详情
    getMoreDetails() {
      this.dialogTableVisible = true;
      let param = {
        Nodeid: this.details_minerId
      };
      this.listLoading = true;
      queryNodeStoredFileListByNodeId(param).then(res => {
        let { result, msg, data } = res;
        if ((result = "ok")) {
          this.listLoading = false;
          this.tableDataMoreDetails = res.data;
        }
      });
    },
    //分页
    handleCurrentChangeJd: function(currentPage) {
      this.currentPageJd = currentPage; //点击第几页
    },
    //改变下拉分页
    handleSizeChangeJd: function(size) {
      this.pagesizeJd = size; //每页下拉显示数据
    },
    //分页
    handleCurrentChangePz: function(currentPage) {
      this.currentPagePz = currentPage; //点击第几页
    },
    //改变下拉分页
    handleSizeChangePz: function(size) {
      this.pagesizePz = size; //每页下拉显示数据
    },
    //分页
    handleCurrentChangeCc: function(currentPage) {
      this.currentPageCc = currentPage; //点击第几页
    },
    //改变下拉分页
    handleSizeChangeCc: function(size) {
      this.pagesizeCc = size; //每页下拉显示数据
    },

    //选择节点发生改变是。checklist发生改变
    handleCheckedChange: function(val) { 
      this.chartData.columns = this.checkList;
    },

    //新增
    createFrom: function() {
      this.dialogCreateVisible = true;
    },

    getDetails: function() {
      this.dialogTableVisible = true;
    },
    handleClick(tab, event) {
      console.log(tab, event);
    },
    //点击表格获取每一行数据
    openDetails(row) {
      this.MapDetailsShow = true;
      let param = {
        NodeId: row.nodeId
      };
      this.listLoading = true;
      queryNodeInfoById(param).then(res => {
        let { result, msg, data } = res;
        if ((result = "ok")) {
          this.listLoading = false;
          this.details_lx = res.data.nodeType;
          this.details_minerId = res.data.minerId;
          this.details_Ip = res.data.ip;
          this.details_nodeState = res.data.nodeState;
          this.details_city = res.data.city;
          this.details_region = res.data.region;
        }
      });
    },
    //获取节点详细信息
    getDetails() {
      // let param = {
      //   CityName: "1个",
      //   NodeType: "矿机节点"
      // };
      //alert(this.checkList)
      var nowparam = this.checkList.slice(1, this.checkList.length);
      if (nowparam.length == 0) {
        this.$message({
          message: "请选择选择一种节点类型",
          type: "warning"
        });
      }
      let param = {
        nodeTypeArr: nowparam
      };
      // alert(JSON.stringify(param))
      this.listLoading = true;
      queryAllNodeProfiesByNodeTypes(param).then(res => {
        let { result, msg, data } = res;
        if ((result = "ok")) {
          this.listLoading = false;
          this.tableDataDetails = res.data.allNodeProfiles;
          this.nodeTypeCount = res.data.totalNodeCnt;
          this.details_length = res.data.length;
          this.chartData.rows = res.data.regionNodeDist;
        }
      });
    },
    getNodeslist() {
      let param = {};
      this.listLoading = true;
      //NProgress.start();
      queryAllConfigRecords(param).then(res => {
        let { result, msg, data } = res;
        if ((result = "success")) {
          this.listLoading = false;
          this.tableDataNodeConfig = res.data;
          this.details_length = res.data.length;
        }
      });
    }
  }
};
</script>

<style  lang="less">
.chart-container {
  // width: 100%;
  float: initial !important;
  min-width: 1450px;

}
.new_grid-content {
  height: auto;
  //background: #999999;
  .item_one_child_r {
    .el-checkbox__inner {
      margin-left: 15px;
    }

    width: 100%;
    height: auto;
    margin: 50px auto;

    overflow: hidden;
    .child_r_b {
      width: auto;
      padding: 0 10px;
      height: 35px;
      line-height: 35px;
      text-align: center;
      margin-top: 30px;
      margin-bottom: 30px;
      float: left;
      border: 1px solid #dfe6ec;
      margin-left: 15px;
      background: #20a0ff;
      color: #ffffff;
      &:nth-child(2) {
        margin-left: 0px;
      }
    }
    .child_r_table {
      .el-table .cell {
        line-height: 18px;
      }
    }
    .el-checkbox + .el-checkbox {
      margin-left: 0px;
      width: 100%;
      margin-top: 10px;
    }
    .child_checkbox {
      width: 100%;
      float: left;
    }
  }
}
.new_grid-content .item_one_child {
  width: 800px;
  height: 600px;
  //background: #666666;
  margin: 50px auto;
}
.new_grid-content {
  .item_three_child {
    width: 800px;
    height: auto;
    overflow: hidden;
    margin-top: 20px;
    margin-bottom: 35px;

    .three_child_load {
      float: left;
      margin-left: 50px;
      .el-button--small {
        padding: 11px 9px;
      }
    }
    .three_child_input {
      float: left;
      width: 350px;
      margin-left: 50px;
    }
    .three_child_button {
      float: left;
    }
  }
  .item_two_child {
    width: 800px;
    height: 600px;

    margin: 50px auto;
    .item_top {
      width: 700px;
      height: 50px;
      margin: 0 auto;
      .item_top_id {
        width: 100px;
        height: 40px;
        line-height: 40px;
        text-align: center;
        float: left;
      }
      .item_top_input {
        width: 500px;
        height: 40px;
        line-height: 40px;
        text-align: center;
        float: left;
      }
      .item_top_button {
        width: 100px;
        height: 40px;
        line-height: 40px;
        text-align: center;
        float: left;
      }
    }
    .item-bottom {
      width: 500px;
      height: auto;
      margin-top: 50px;
      margin: 100px auto;
      background: #e4e8f1;
      padding-top: 15px;

      p {
        width: 100%;
        text-align: center;
        font-size: 14px;
      }
    }
  }
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
  margin-top: 0px;
  .item_title_child {
    width: 100%;
    margin: 0 auto;
    height: auto;
    background: #f2f2f2;
    overflow: hidden;
    text-align: center;
  }
}
.customWidth1 {
  width: 80%;
}
.div_MapDetails {
  width: 200px;
  height: 250px;
  background: #999999;
  position: absolute;
  top: 100px;
  z-index: 1;
  border-radius: 5px;
  p {
    text-indent: 10px;
  }
}
.child_r_t .el-button--primary {
  margin-left: 15px;
}
</style>