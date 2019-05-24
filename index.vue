<template  >

  <el-container >
    <el-header >
      <div class = "head" >
        <el-switch v-model = "value2" active-color = "#36a3f7"
                   inactive-color = "#ff4949" ></el-switch >
        公告：
      </div >
    </el-header >
    <el-main >
      <div class = "main" >
        <table >
          <tr >
            <h3 >个人信息</h3 >
          </tr >
          <tr >
            <td >商户名称：{{name}}</td >
            <td >商户编号：{{num}}</td >
          </tr >
          <tr >
            <td >商户状态：</td >
            <td ></td >
          </tr >
          <tr >
            <td >上级商户编号：</td >
          </tr >
          <tr >
            <td >本周提现次数：</td >
            <td >最后提现时间：</td >
          </tr >
          <tr >
            <td >账户保证金：</td >
          </tr >
          <tr >
            <td >
              转账账户：
              <el-select v-model = "value" placeholder = "请选择" >
                <el-option
                  v-for = "item in options"
                  :key = "item.value"
                  :label = "item.label"
                  :value = "item.value"
                ></el-option >
              </el-select >
              <el-button type = "primary" >编辑银行用户</el-button >
            </td >
            <td >
            
            </td >
          </tr >
          <tr >
            <td >数据最后一次更新时间：</td >
            <td ></td >
          </tr >
        </table >
        <div class = "table" id = "app" >
          <tr >
            <td ><h3 >账目信息（每日结算表金额）</h3 ></td >
          </tr >
          <table >
            <thead >
            <tr >
              <td >#</td >
              <td >现金</td >
              <td >银行卡</td >
              <td >支付宝</td >
              <td >微信</td >
              <td >代收现金</td >
              <td >小计</td >
              <td style = "text-align: right" >
                <el-button type = "primary" style = "" >刷新数据</el-button >
              </td >
            </tr >
            </thead >
            <tbody v-for = "(item,index) in tableData" :key = "index" >
            <tr >
              <td >昨日结算</td >
              <td >{{item.cash}}</td >
              <td >{{item.bankCard}}</td >
              <td >{{item.alipay}}</td >
              <td >{{item.weChat}}</td >
              <td >{{item.cashCollection}}</td >
              <td >{{item.sum}}</td >
              <td ></td >
            </tr >
            <tr >
              <td >结算累计</td >
              <td >{{item.allCash}}</td >
              <td >{{item.allBankCard}}</td >
              <td >{{item.allAlipay}}</td >
              <td >{{item.allWeChat}}</td >
              <td >{{item.allCashCollection}}</td >
              <td >{{item.allSum}}</td >
              <td ></td >
            </tr >
            </tbody >
          </table >
        </div >
        <div class = "table" >
          <span><h3 >帐户资金记录</h3 ></span>
          <hr style="width: 98%;border:0.5px solid #eaeaea;">
          <table class = "count" >
            <td >
              <el-date-picker
                v-model = "data1"
                type = "date"
                placeholder = "选择日期" >
              </el-date-picker >
              至
              <el-date-picker
                v-model = "data2"
                type = "date"
                placeholder = "选择日期" >
              </el-date-picker >
              <el-select v-model = "value" placeholder = "请选择" >
                <el-option
                  v-for = "item in countData"
                  :key = "item.value"
                  :label = "item.label"
                  :value = "item.value" >
                </el-option >
              </el-select >
            </td >
            <td><el-button type = "primary" size="medium" >刷新数据</el-button ></td>
            <el-divider ></el-divider>
          </table >
          <div class="count">
            <el-table
              :data="accountRecord"
              :default-sort = "{prop: 'date', order: 'descending'}"
            >
              <el-table-column
                prop="time"
                label="时间"
                sortable
                width="260">
              </el-table-column>
              <el-table-column
                prop="InAmount"
                label="转入金额"
                sortable
                width="260">
              </el-table-column>
              <el-table-column
                prop="OutAmount"
                label="转出金额"
                sortable
                width="260">
              </el-table-column>
              <el-table-column
                prop="OriginalAmount"
                label="原金额"
                sortable
                width="260">
              </el-table-column>
              <el-table-column
                prop="CurrentAmount"
                label="当前金额"
                sortable
                width="260">
              </el-table-column>
              <el-table-column
                prop="ChangeType"
                label="变更类型"
                sortable
                width="260">
              </el-table-column>
              <el-table-column
                prop="ChangeDescription"
                label="变更说明"
                sortable
                width="260">
              </el-table-column>
            </el-table>
            <div class="block">
              <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="currentPage4"
                :page-sizes="[100, 200, 300, 400]"
                :page-size="100"
                layout="total, sizes, prev, pager, next, jumper"
                :total="400">
              </el-pagination>
            </div>
          </div>
        </div >
        <div class="table">
          <span><h3 >提现申请</h3 ></span>
          <hr style="width: 98%;border:0.5px solid #eaeaea;">
          <ol style="list-style: none;color: red;margin-left: -20px">
            <li>注意</li>
            <li>1.提现金额必须大于500元</li>
            <li>2.提现金额必须为整数</li>
            <li>3.每周提现次数不能超过两次 </li>
          </ol>
          <hr style="width: 98%;border:0.5px solid #eaeaea;">
          <p style="margin-left: 20px;">当前提现金额：</p>
          <hr style="width: 98%;border:0.5px solid #eaeaea;">
          <el-form label-width="100px">
            <el-form-item label="提现金额:" required="" prop="">
              <el-input style="width:20%;" placeholder="请输入提现金额" ></el-input>
              <el-button type="primary" style="margin-left: 20px">申请提现</el-button>
            </el-form-item>
          </el-form>
        </div>
        <hr style="width: 98%;border:0.5px solid #eaeaea;">
        <div class="table">
          <span><h3 >提现记录</h3 ></span>
          <hr style="width: 98%;border:0.5px solid #eaeaea;">
          <div style="margin-top: 30px;margin-left: 20px;min-width:100%;">
          <td style="">
          <el-date-picker
            v-model="value1"
            type="date"
            placeholder="选择日期">
          </el-date-picker>至
          <el-date-picker
            v-model="value1"
            type="date"
            placeholder="选择日期">
          </el-date-picker>
          </td>
          <td style="padding-left:10px">
          <el-input placeholder="请输入申请订单" style="width: 300px" >
            <template slot="prepend">申请订单</template>
          </el-input>
        </td>
          <td style="padding-left:10px">
            申请状态：
          <el-select v-model="value" placeholder="请选择">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select></td>
          <td style="padding-left: 30px"><el-button type="primary" style="margin-left: 20px">刷新数据</el-button></td>
           </div>
          <div STYLE="text-align: center">
          <el-table
            :data="withdrawData"
            style="width: 100%;margin-bottom: 20px; margin-left:20px;  "
            row-key="id">
            <el-table-column
              prop="applicationDate"
              label="申请时间"
              sortable
              width="180" header-align="center">
            </el-table-column>
            <el-table-column
              prop="applicationNum"
              label="申请单号"
              sortable
              width="180" header-align="center">
            </el-table-column>
            <el-table-column
              prop="cashWithdrawalAmount"
              sortable
              width="180"
              label="提现金额" header-align="center">
            </el-table-column>
            <el-table-column
              prop="commission"
              label="手续费"
              sortable
              width="180" header-align="center">
            </el-table-column>
            <el-table-column
              prop="totalDeductibleAmount"
              label="提现总扣除金额"
              sortable
              width="180" header-align="center">
            </el-table-column>
            <el-table-column
              prop="status"
              sortable
              width="180"
              label="申请状态" header-align="center">
            </el-table-column>
            <el-table-column
              prop="auditorAccount"
              label="审核人账号"
              sortable
              width="180" header-align="center">
            </el-table-column>
            <el-table-column
              prop="auditTime"
              label="审核时间"
              sortable
              width="180" header-align="center">
            </el-table-column>
            <el-table-column
              prop="remarks"
              sortable
              width="180"
              label="备注" header-align="center">
            </el-table-column>
          </el-table>
          
          </div>
          <div class="block">
            <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="currentPage4"
              :page-sizes="[100, 200, 300, 400]"
              :page-size="100"
              layout="total, sizes, prev, pager, next, jumper"
              :total="400">
            </el-pagination>
          </div>
        </div>
      </div >
    </el-main >
  </el-container >
</template >

<script >
  export default {
    data () {
      return {
        value1: true,
        value2: true,
        data1: '',
        data2: '',
        options: [
          {
            value: '选项1',
            label: '黄金糕'
          },
          {
            value: '选项2',
            label: '双皮奶'
          },
          {
            value: '选项3',
            label: '蚵仔煎'
          },
          {
            value: '选项4',
            label: '龙须面'
          },
          {
            value: '选项5',
            label: '北京烤鸭'
          }
        ],
        value: '',
        name: '',
        num: '',
        tableData: [{
          cash: '12',
          bankCard: '11',
          alipay: '234',
          weChat: '3.2',
          cashCollection: '10',
          sum: '10',
          allCash: '12',
          allBankCard: '22',
          allAlipay: '11',
          allWeChat: '33',
          allCashCollection: '34',
          allSum: '324'
        }
        ],
        countData: [{
          value: '1',
          label: '全部'
        },
          {
            value: '2',
            label: '双皮奶'
          },
          {
            value: '3',
            label: '蚵仔煎'
          },
          {
            value: '4',
            label: '龙须面'
          },
          {
            value: '5',
            label: '北京烤鸭'
          }
        ],
        accountRecord:[{
          time:'1s1',
          InAmount:'幅度幅度不大不小这次行程v',
          OutAmount:'士大夫似的是广东省',
          OriginalAmount:'吃饭v的得分得分得分的',
          CurrentAmount:'发的风格的同意让她',
          ChangeType:'惹她给v部分的',
          ChangeDescription:'人体一日546他闰土闰土股份',
        }],
         withdrawData:[{
          applicationDate:'wqwq',
           applicationNum:'xasxa',
           cashWithdrawalAmount:'ass',
           commission:'fdff',
           totalDeductibleAmount:'WWWW',
           status:'sdsd',
           auditorAccount:'sdsds',
           auditTime:'cxcx',
           remarks:'feredf'
        }]
      }
    }
  }
</script >
<style scoped = "scoped" >
  hr{
    width: 98%;border:0.5px solid #eaeaea;
  }
  .el-header {
    background-color: #e9f3ff;
    color: #333;
    text-align: left;
    line-height: 30px;
  }
  
  .el-main {
    background-color: #e9eef3;
    color: #333;
    text-align: left;
  }
  
  .head {
    margin-top: 20px;
  }
  
  .el-switch {
    margin-right: 10px;
  }
  
  .main {
    background-color: #ffffff;
    
  }
  
  .main table {
    border-collapse: collapse;
    margin-left: 15px;
    width: 800px;
  }
  
  .main table tr {
    border: 1px solid #eaeaea;
    border-right: none;
    border-left: none;
    line-height: 50px;
  }
  
  .main table tr h2 {
    padding-top: 20px;
  }
  
  .main table tr td {
    width: 1200px;
  }
  
  .table {
    margin-top: 50px;
    margin-left: 5px;
  }
  
  .table tr td {
    border: 1px solid #eaeaea;
    border-right: none;
    border-left: none;
    border-top: none;
    text-align: center;
    padding-left: 18px;
  }
  .table span h3 {
    margin-left:20px;
  }
  .count{
    width: 98%;
  }
  .count .el-table {
    margin-left: 20px;
  }
  .count .el-table .el-table-column {
    width: 100%;
  }
.block{
  margin-left: 20px;
}
</style >

