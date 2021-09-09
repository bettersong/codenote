```js
<template>
  <div>
    <el-form
      ref="ruleForm"
      :model="formData"
      :rules="rules"
      label-width="160px"
      v-loading="loading"
      :disabled="this.isEdit === '查看'"
    >
      <el-form-item label-width="0">
        <el-button type="primary" disabled>
          <span v-if="+this.formData.push_id > 0">编辑推送信息</span>
          <span v-else>新增推送信息</span>
        </el-button>
        <!--<el-button @click="goUserGroup">用户分组</el-button>-->
      </el-form-item>

      <el-form-item label="id：" v-if="+formData.push_id > 0" v-show="this.isEdit !='复制'">
        <span>{{formData.push_id}}</span>
      </el-form-item>

        <el-form-item label="产品端:" required>
          <el-radio-group v-model="formData.app">
            <el-radio :label="2">米读</el-radio>
            <el-radio :label="4">极速</el-radio>
          </el-radio-group>
        </el-form-item>
      <el-form-item label="推送类型：" style="margin-bottom: 5px;">
        <el-select
          :disabled="+formData.push_id > 0"
          v-model="formData.isTemplateMessage"
          placeholder="请选择推荐类型"
        >
          <el-option
            v-for="item in config.isTemplateMessage"
            :key="item.k"
            :label="item.v"
            :value="item.k"
          ></el-option>
        </el-select>
      </el-form-item>

      <el-form-item label="标题：" prop="title">
        <el-input v-model="formData.title" placeholder="标题" ></el-input>
        <span style="color: #999999;">{{formData.title.length}}/32</span>
      </el-form-item>

      <el-form-item label="推送内容：" prop="content">
        <el-input
          placeholder="推送内容"
          style="width: 300px;"
          type="textarea"
          v-model="formData.content"
        ></el-input>
        <span style="color: #999999;">{{formData.content.length}}/100</span>
      </el-form-item>
      <el-form-item label="是否有奖励：">
        <el-checkbox-group v-model="enabledArrData">
          <el-checkbox
            v-for="item in enabledArr"
            :key="item.id"
            :label="item.id"
            :value="item.id"
          >{{item.name}}</el-checkbox>
        </el-checkbox-group>
      </el-form-item>
      <el-form-item label="金币数量：">
        <el-input-number v-model="reward.extra.min_coin" :min="1"></el-input-number>
        ～
        <el-input-number v-model="reward.extra.max_coin" :min="1"></el-input-number>
      </el-form-item>
      <el-form-item label="阅读时间：">
        <el-input-number v-model="reward.extra.receive_limitations.read_time" :min="1"></el-input-number>
      </el-form-item>
      <version-list-v2
        @emitData="emitAppData"
        ref="versionListV2"
        :appId="appId">
      </version-list-v2>
      <el-form-item label="落地页：" style="margin-bottom: 5px;">
        <el-radio-group v-model="formData.redirect">
          <el-radio
            v-for="item in config.redirect"
            :key="item.k"
            :label="item.k"
            :value="item.k"
          >{{item.v}}</el-radio>
        </el-radio-group>
      </el-form-item>

      <!--固定选择的落地页面-->
      <div>
        <el-form-item label prop="redirectLoadingPage" v-if="formData.redirect === 'fix'">
          <landingFix
            @onLandingHandle="onLandingHandle"
            :pageVal="formData.redirectLoadingPage"
            :pageList="config.redirectLoadingPage"
          ></landingFix>
        </el-form-item>
      </div>

      <!--专题落地页面-->
      <div>
        <el-form-item label prop="name" v-if="formData.redirect === 'specialTopic'">
          <landingSpecialTopic @onLandingHandle="onLandingHandle" :name="formData.name"></landingSpecialTopic>
        </el-form-item>
      </div>

      <!--自定义落地页面-->
      <div>
        <el-form-item label prop="url" v-if="formData.redirect === 'h5'">
          <el-input v-model="formData.url" placeholder="请输入链接"></el-input>
        </el-form-item>
      </div>

      <!--书籍落地页-->
      <div>
        <el-form-item label prop="book" v-if="formData.redirect === 'book'">
          <landingBook @onLandingHandle="onLandingHandle" :book="formData.book"></landingBook>
        </el-form-item>
      </div>

      <!--推送对象-->
      <el-row>
        <el-col :span="12">
          <pushObject
            @onPushObjSelect="onPushObjSelect"
            @onPushObjHandle="onPushObjHandle"
            :tags.sync="tags"
            :selectTags.sync="formData.selectTags"
            :formDataTarget="formData.target"
            :groupName="formData.groupName"
            :downloadTime="formData.downloadTime"
            :recentlyStartTime="formData.recentlyStartTime"
            :userGroupList="userGroupList"
            :fileList="formData.fileList"
            :configTarget="config.target"
          ></pushObject>
        </el-col>
        <el-col :span="8">
          <el-form-item label="是否使用最新数据:">
            <el-select v-model="formData.isTimely" placeholder="请选择" style="width: 100px;">
              <el-option
                v-for="item in isTimely"
                :key="item.k"
                :label="item.v"
                :value="item.k">
              </el-option>
            </el-select>
          </el-form-item>
        </el-col>
      </el-row>
      <el-form-item v-if="formData.target === 'file'" label="文件链接：" prop="userFile">
        <el-input v-model="formData.userFile" placeholder="文件链接"></el-input>
      </el-form-item>

      <el-form-item label="标签集合：" v-if="formData.target === 'tag'">
        <el-radio-group v-model="formData.tagType">
          <el-radio
            v-for="item in tagType"
            :key="item.id"
            :label="item.id"
            :value="item.id"
          >{{item.name}}</el-radio>
        </el-radio-group>
        <p v-show="formData.tagType === 1" style="color: #ff0000;">交集指拥有所选标签相同属性的用户群</p>
        <p v-show="formData.tagType === 2" style="color: #ff0000;">并集指所选用户标签下的合集并去重</p>
      </el-form-item>

      <el-form-item label="排除对象：">
        <el-radio-group v-model="formData.is_ex_target">
          <el-radio
              v-for="item in config.isEx"
              :key="item.k"
              :label="item.k"
              :value="item.k"
          >{{item.v}}</el-radio>
        </el-radio-group>
      </el-form-item>

      <!--排除对象-->
      <template v-if="+formData.is_ex_target === 1">
        <el-row>
        <el-col :span="12">
        <pushObjectEx
            @onPushObjSelect="onPushObjSelectEx"
            @onPushObjHandle="onPushObjHandleEx"
            labelTitle="排除对象："
            :formDataTarget="formData.ex_target"
            :groupName="formData.ex_groupName"
            :userGroupList="userGroupList"
            :fileList="formData.ex_fileList"
            :configTarget="config.exTarget"
        ></pushObjectEx>
        </el-col>
        <el-col :span="8">
          <el-form-item label="是否使用最新数据:">
            <el-select v-model="formData.ex_isTimely" placeholder="请选择" style="width: 100px;">
              <el-option
                v-for="item in ex_isTimely"
                :key="item.k"
                :label="item.v"
                :value="item.k">
              </el-option>
            </el-select>
          </el-form-item>
        </el-col>
        </el-row>
        <el-form-item v-if="formData.ex_target === 'file'" label="文件链接：" prop="ex_userFile">
          <el-input v-model="formData.ex_userFile" placeholder="文件链接"></el-input>
        </el-form-item>
      </template>

      <el-form-item label="推送组别: " prop="rateAb">
        <el-checkbox-group v-model="formData.rateAb">
          <el-checkbox
            v-for="item in config.abConfig"
            :key="item.k"
            :label="item.k"
            :value="item.k"
          >{{item.v}}</el-checkbox>
        </el-checkbox-group>
        <div style="color: red;" v-show="showAbWarning">提示: 为测试推送对用户行为的影响, 对照A一般不推送消息, 请慎重选择</div>
      </el-form-item>

      <el-form-item label="推送时间：" prop="pickerDateTime" v-if="formData.isTemplateMessage === 0">
        <el-col>
          <el-date-picker
            v-model="formData.pickerDateTime"
            value-format="yyyy-MM-dd HH:mm:ss"
            type="datetimerange"
            range-separator="至"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
          ></el-date-picker>
        </el-col>
      </el-form-item>

      <el-form-item label="推送时间：" v-else>
        <el-col>
          <el-time-picker
            is-range
            v-model="formData.pickerDateTime"
            range-separator="-"
            start-placeholder="开始时间"
            end-placeholder="结束时间"
            placeholder="选择时间范围"
          ></el-time-picker>
        </el-col>
      </el-form-item>

      <el-form-item label="是否启用：" v-if="+formData.isTemplateMessage === 1">
        <el-radio-group v-model="formData.startState">
          <el-radio
            v-for="item in config.startState"
            :key="item.k"
            :label="item.k"
            :value="item.k"
          >{{item.v}}</el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="是否开启角标：">
        <el-radio-group v-model="formData.is_badge_enabled">
          <el-radio label="1">开启</el-radio>
          <el-radio label="0">关闭</el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="上传视频/大图片:">
        <el-radio-group v-model="videoAndPic">
          <el-radio :label="1">视频</el-radio>
          <el-radio :label="2">大图片</el-radio>
        </el-radio-group>
        <template v-if="videoAndPic == 1">
          <div>
            <el-input v-model="formData.notify_img_url.notify_video" clearable></el-input>
            <uploadVideo class="dp-i-block" @emitUrl="emitViedoUrl"></uploadVideo>
            <div class="tips" style="color:red; font-size:12px;">当前只支持mp4格式，如需扩展请联系产品</div>
          </div>
        </template>
        <template v-else class="showPic">
          <uploadImg
            :picFile="formData.notify_img_url.notify_picture"
            :picType="'banner'"
            @emitPicFile="changeIconBig"
          />
        </template>
      </el-form-item>
      <!-- <el-form-item label="大图片:" class="showPic">
        <uploadImg
          :picFile="formData.notify_img_url.notify_picture"
          :picType="'banner'"
          @emitPicFile="changeIconBig"
        />
      </el-form-item> -->
      <el-form-item label="小图片:" class="showPic">
        <uploadImg
          :picFile="formData.notify_img_url.notify_icon"
          :picType="'banner'"
          @emitPicFile="changeIcon"
        />
      </el-form-item>
      <el-form-item>
        <el-button v-if="+this.formData.push_id > 0" type="danger" @click="deletePush">删除这条推送信息</el-button>
        <el-button @click="goPushList">取消</el-button>
        <el-button type="primary" @click="onSubmit('ruleForm')">保存</el-button>
      </el-form-item>
    </el-form>
    <el-row class="cancel">
      <el-button v-if="this.isEdit === '查看'" type="primary" @click="goPushList">返回</el-button>
    </el-row>
  </div>
</template>

<script>
import formRules from "./js/push-add-form-rules";
import pushObject from "../components/push/push-object/index";
import pushObjectEx from "../components/push/push-object-ex/index";
import landingFix from "../components/push/landing-fix";
import landingBook from "../components/push/landing-book";
import { partsPostData, partsDetailData } from "./js/common";
import { setDetailData, getDetailData } from "./js/detailStorage";
import landingSpecialTopic from "../components/push/landing-special-topic";
import QuRequest from "@/core/QuRequest";
import versionListV2 from "@/components/dtu-version-list-v2/version";
import uploadImg from '@/components/uploadImg.vue';
import uploadVideo from '@/components/uploadVideo.vue';

const quRequest = new QuRequest();

export default {
  name: "userGroupAutoPush",
  components: {
    pushObject,
    versionListV2,
    pushObjectEx,
    landingFix,
    landingSpecialTopic,
    landingBook,
    uploadImg,
    uploadVideo
  },
  data() {
    return {
      appId: 1,
      loading: false,
      // 表单信息,
      enabledArr: [
        { id: 1, name: '阅读领奖励' }
      ],
      enabledArrData: [],
      reward: {
        type: 1,
        enable: true,
        extra: {
          min_coin: 1,
          max_coin: 2,
          receive_limitations: {
            read_time: 1
          }
        }
      },
      formData: {
        rateAb: ['b', 'c'],
        app: 2,
        target: "", // 推送对象
        push_id: this.$route.query.id,
        title: "",
        content: "",
        platform: [2],
        redirect: "fix", // 落地页类型
        redirectLoadingPage: null, // 固定落地页id
        name: "", // 落地专题名称
        specialTopicId: "", // 落地专题id
        url: "", // 自定义落地页的url
        book: "", // 落地书籍
        hash_id: "", // 落地书籍id
        pickerDateTime: [], // 推送时间
        startState: 1, // 是否启用
        groupName: "", // 分组名字
        groupId: null, // 分组用户的分组id
        tagType: 2, // tag集合类型
        selectTags: [],
        userFile: "", // 特定用户标签上传文件
        downloadTime: [], // 下载时间
        recentlyStartTime: [this.setDefaultDate(-90), this.setDefaultDate(0)], // 最近启动时间
        fileList: [], // 上传文件列表
        isTemplateMessage: 0,
        is_ex_target: 0, // 是否需要排除推送对象
        ex_target: "", // 排除推送对象
        ex_fileList: [], // 排除推送对象文件
        ex_groupName: "", // 排除对象分组名字
        ex_groupId: null, // 排除对象分组用户的分组id
        ex_userFile: "", // 排除对象特定标签上传的文件
        isTimely: true,
        ex_isTimely: true,
        is_badge_enabled: "0",
        notify_img_url: {
          notify_picture: "",
          notify_icon: "",
          notify_video: ""
        }
      },
      videoAndPic: 1,
      // 配置信息
      config: {
        platform: [],
        redirect: [],
        redirectLoadingPage: [],
        startState: [],
        target: [],
        isTemplateMessage: [],
        isEx: [],
        exTarget: [],
      },
      isTimely: [{ k: true, v: '是' }, { k: false, v: '否' }],
      ex_isTimely: [{ k: true, v: '是' }, { k: false, v: '否' }],
      tagType: [{ id: 1, name: "交集" }, { id: 2, name: "并集" }],
      tags: [], // tag列表
      userGroupList: [], // 分组列表
      rules: formRules, // 表单验证规则
      isEdit: this.$route.query.isEdit
    };
  },
  computed: {
    showAbWarning() {
      const { rateAb } = this.formData
      if (rateAb) {
        for (const item of rateAb) {
          if (item == 'a') {
            return true
          }
        }
      }
      return false
    }
  },
  watch: {
    "formData.isTemplateMessage": function(val, oldval) {
      if (!this.formData.push_id) {
        if (val === 0) {
          this.formData.pickerDateTime = [];
        } else {
          this.formData.pickerDateTime = [
            new Date(new Date().toLocaleDateString()).getTime(),
            new Date(new Date().toLocaleDateString()).getTime() +
              (24 * 60 * 60 * 1000 - 1)
          ];
        }
      }
    },
    "formData.target": function(val, oldval) {
      if (val === "group" && !this.formData.push_id) {
        console.log(1111);
      }
    }
  },
  mounted() {
    this.$refs.versionListV2.getConfig()
    if (+this.formData.push_id > 0) {
      this.getDetailRequest();
    } else {
      // 新增，不需要默认填充数据
      this.loading = true;
      this.getConfig().then(res => {
        this.getTags().then(tag => {
          this.loading = false;
          let pushAddStorage = sessionStorage.getItem("pushAddStorage");
          if (pushAddStorage === null || pushAddStorage === "null") { return false; }
          let detail = JSON.parse(pushAddStorage);

          this.formData = getDetailData(this.tags, this.formData, detail);
        });
      });
    }
  },
  methods: {
    emitAppData (data) {
      console.log(data)
    },
    changeIcon(url) {
      this.formData.notify_img_url.notify_icon = url;
    },
    changeIconBig(url) {
      this.formData.notify_img_url.notify_picture = url;
    },
    emitViedoUrl(url) {
      console.log(url)
      this.formData.notify_img_url.notify_video = url
    },
    deletePush() {
      this.$confirm(`确认删除？`, "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          quRequest
            .send(quRequest.Apis.qukan.push.NotificationPush_Del, {
              id: this.formData.push_id
            })
            .then(r => {
              if (r.code === 0) {
                this.$message({
                  message: "删除成功",
                  type: "success"
                });
                this.goPushList();
              } else {
                this.$message({
                  message: r.message,
                  type: "success"
                });
              }
            })
            .catch(err => console.log(err));
        })
        .catch(() => {
          // do nth
        });
    },
    addZero(data) {
      return data < 10 ? "0" + data : data;
    },
    setDefaultDate(day) {
      // 设置X天后的日期时间
      let timeStamp = new Date().setDate(new Date().getDate() + day);
      const date = new Date(timeStamp);
      const yy = date.getFullYear();
      const mm = date.getMonth() + 1;
      const dd = date.getDate();
      return `${yy}-${this.addZero(mm)}-${this.addZero(dd)} 00:00:00`;
    },
    goPushList() {
      sessionStorage.setItem("pushAddStorage", "null");
      this.$router.push({
        path: "push-list"
      });
    },
    goUserGroup() {
      let pushAddStorage = setDetailData(this.formData);
      sessionStorage.setItem("pushAddStorage", JSON.stringify(pushAddStorage));
      this.$router.push({
        path: "user-group"
      });
    },
    // 对象选择下拉回调
    onPushObjSelect(val) {
      this.formData.target = val;
    },
    // 推送对象组件回调
    onPushObjHandle(val) {
      switch (val.type) {
      case "downloadTime": // 全部用户 - 下载时间
        this.formData.downloadTime = val.data;
        break;
      case "recentlyStartTime": // 全部用户 - 最近启动时间
        this.formData.recentlyStartTime = val.data;
        break;
      case "tag": // 标签用户
        if (val.data.length <= 0) return false;
        this.formData.selectTags = val.data;
        this.$refs.ruleForm.clearValidate(["selectTags"]); // 清理表单验证
        break;
      case "group": // 分组用户
        this.formData.groupId = +val.data.id;
        this.formData.groupName = val.data.name;
        this.$refs.ruleForm.clearValidate(["groupName"]); // 清理表单验证
        break;
      case "file": // 特定用户
        this.formData.userFile = val.data;
        this.$refs.ruleForm.clearValidate(["userFile"]); // 清理表单验证
        break;
      }
    },
    // 排除对象选择下拉回调
    onPushObjSelectEx(val) {
      this.formData.ex_target = val;
    },
    // 排除对象组件回调
    onPushObjHandleEx(val) {
      switch (val.type) {
      case "group": // 分组用户
        this.formData.ex_groupId = +val.data.id;
        this.formData.ex_groupName = val.data.name;
        this.$refs.ruleForm.clearValidate(["ex_groupName"]); // 清理表单验证
        break;
      case "file": // 特定用户
        this.formData.ex_userFile = val.data;
        this.$refs.ruleForm.clearValidate(["ex_userFile"]); // 清理表单验证
        break;
      }
    },
    // 落地页组件监听事件
    onLandingHandle(res) {
      this.$refs.ruleForm.clearValidate([
        "redirectLoadingPage",
        "name",
        "url",
        "book"
      ]); // 清理表单验证
      switch (res.type) {
      case "fix": // 固定页面
        this.formData.redirectLoadingPage = res.data;
        break;
      case "specialTopic": // 专题
        this.formData.name = res.data.value;
        this.formData.specialTopicId = res.data.id;
        break;
      case "book": // 书籍
        this.formData.book = res.data.value;
        this.formData.hash_id = res.data.hashId;
        break;
      default:
        console.warn("onLandingHandle");
      }
    },
    // 获取标签
    getTags() {
      return new Promise((resolve, reject) => {
        resolve({});
        // getTags接口后端说不用了，让删除。先注释，说不定哪天他们又要用呢
        // quRequest
        //   .send(quRequest.Apis.qukan.push.getTags)
        //   .then(res => {
        //     this.tags = res.data.tagList;
        //     this.config.tagType = res.data.type;
        //     resolve(res);
        //   })
        //   .catch(err => {
        //     this.$message.error(`获取失败: ${err.message}`);
        //     throw err;
        //   })
        //   .done();
      });
    },
    // 获取配置
    getConfig() {
      return new Promise(resolve => {
        quRequest
          .send(quRequest.Apis.qukan.push.NotificationPush_Cfg)
          .then(res => {
            this.config = res.data;
            this.config.startState = res.data.templateMessageStatus;
            resolve(res);
          })
          .catch(err => {
            this.$message.error(`获取失败: ${err.message}`);
            throw err;
          })
          .done();
      });
    },
    // 获取数据详情
    getDetail() {
      return new Promise(resolve => {
        // tag列表
        this.getTags().then(tag => {
          // 详情
          quRequest
            .send(quRequest.Apis.qukan.push.NotificationPush_Get, {
              id: this.formData.push_id
            })
            .then(res => {
              resolve(res);
            })
            .catch(err => {
              this.$message.error(`获取失败: ${err.message}`);
              throw err;
            })
            .done();
        });
      });
    },
    getDetailRequest() {
      this.loading = true;
      this.getConfig().then(res => {
        // 填充默认数据
        this.getDetail().then(r => {
          this.formData = partsDetailData(this.tags, this.formData, r.data);
          this.loading = false;
          this.formData.is_badge_enabled = r.data.is_badge_enabled.toString();
          this.formData.notify_img_url = r.data.notify_img_url;
          this.reward = r.data.reward
          if (this.reward.enable) {
            this.enabledArrData = [1]
          }
          console.log(this.reward)
          // 判断
          const notify_picture = this.formData.notify_img_url.notify_picture
          const jpg = notify_picture.indexOf('.jpg') != -1
          const png = notify_picture.indexOf('.png') != -1
          const gif = notify_picture.indexOf('.gif') != -1
          this.videoAndPic = (jpg || png || gif) ? 2 : 1
          console.log(this.videoAndPic, notify_picture)
          this.formData.notify_img_url.notify_video = notify_picture
        });
      });
    },
    // 表单验证
    onSubmit(formName) {
      // 处理是否有奖励数据
      console.log(this.reward)
      this.reward.enable = this.enabledArrData.length > 0
      this.formData.reward = this.reward
      // return
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.update();
        } else {
          this.$message.error("请填写完整的表单信息~");
          return false;
        }
      });
    },
    // 更新数据
    update() {
      this.loading = true;
      if (this.$route.query.isEdit === '复制') {
        this.formData.push_id = '';
      }
      if (this.videoAndPic == 1) {
        this.formData.notify_img_url.notify_picture = this.formData.notify_img_url.notify_video
      }
      delete this.formData.notify_img_url.notify_video // 删除上传视频节点.

      // 提交接口
      quRequest
        .send(
          quRequest.Apis.qukan.push.NotificationPush_Save,
          partsPostData(this.formData)
        )
        .then(res => {
          if (res.code === 0) {
            this.$message({
              message: "保存成功",
              type: "success"
            });
            sessionStorage.setItem("pushAddStorage", "null");
            this.goPushList();
          }
          this.loading = false;
        })
        .catch(err => {
          this.$message.error(`提交失败: ${err.message}`);
          throw err;
        })
        .done();
    }
  }
};
</script>

<style scoped>
.el-input,
.el-select,
.el-autocomplete {
  width: 300px;
}
.cancel{
  margin: 30px 70px;
}
</style>

```

```js
<style lang="less">
.rate-input.el-input {
  width: 100px;
}

.d-i-block {
  display: inline-block;
}

.rate-ab-list {
  padding: 10px 5px;
  border: 1px solid #dcdfe6;
  margin-bottom: 10px;
}
</style>

<template>
  <div class="box">
    <el-form
      ref="form"
      :model="formData"
      label-width="120px"
      class="config"
      v-loading="loading"
      :rules="rules"
    >
      <el-form-item label="产品端:" required>
        <el-radio-group v-model="appId" @change="changeApp">
          <el-radio v-for="item in config.appId" :key="item.k" :label="item.k">{{item.v}}</el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="所属页面:" prop="page">
        <el-select v-model.trim="formData.page" placeholder="请选择页面">
          <el-option
            v-for="(item, index) in config.web"
            :key="index"
            :label="item.v"
            :value="item.k"
            :disabled="queryId != ''"
          ></el-option>
        </el-select>
      </el-form-item>

      <el-form-item label="运营页位置:" v-show="configPosition.length > 0" prop="action">
        <el-radio
          v-for="item in configPosition"
          :key="item.k"
          v-model="formData.action"
          :label="item.k"
          :disabled="queryId != ''"
        >{{item.v}}</el-radio>
      </el-form-item>

      <el-form-item :label="formItem.nameLabel" prop="name">
        <el-input
          v-model.trim="formData.name"
          style="width: 300px"
          :placeholder="'请填写' + formItem.nameLabel"
        ></el-input>
      </el-form-item>
      <template v-if="formData.action === 4">
        <el-form-item label="阅读偏好：">
          <el-checkbox-group v-model="formData.sex">
            <el-checkbox
              v-for="version in config.sex"
              :key="version.k"
              :label="version.k"
            >{{version.v}}</el-checkbox>
          </el-checkbox-group>
        </el-form-item>
      </template>
      <template v-else>
        <el-form-item label="物料:" prop="picFile" class="showPic">
          <uploadImg :picFile="formData.picFile" :picType="'banner'" @emitPicFile="changeIcon" />
        </el-form-item>
      </template>

      <el-form-item label="跳转页面：" required prop="type" v-if="appId === 2">
        <!--  @change="changeWay" -->
        <el-radio-group v-model="formData.type">
          <el-radio v-for="item in config.urlto" v-show="item.k !== 5" :key="item.k" :label="item.k" :value="item.k">
            <span
              v-if="item.k === 2"
            >{{item.v + '(cpc webview请在链接后面加 useCpcWebview=true Demo( https://baidu.com?useCpcWebview=true))'}}</span>
            <span v-else>{{item.v}}</span>
          </el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="跳转页面：" required prop="type" v-else>
        <el-radio-group v-model="formData.type">
          <el-radio v-for="item in config.urlto" v-show="item.k !== 2" :key="item.k" :label="item.k" :value="item.k">
            <span
              v-if="item.k === 2"
            >{{item.v + '(cpc webview请在链接后面加 useCpcWebview=true Demo( https://baidu.com?useCpcWebview=true))'}}</span>
            <span v-else>{{item.v}}</span>
          </el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item v-if="formData.type === 1">
        <queryBook :list="bookList" @emitBooks="emitBooks" />
      </el-form-item>

      <el-form-item v-else-if="formData.type === 2 || formData.type === 4 || formData.type === 5">
        <el-input
          type="textarea"
          rows="4"
          v-model.trim="formData.url"
          style="width: 300px;"
          placeholder="请输入url"
        ></el-input>
      </el-form-item>

      <el-form-item v-else-if="formData.type === 3">
        <el-select v-model="formData.nativeUrl" placeholder="请选择页面">
          <el-option
            v-for="(item, index) in config.nativePage"
            :key="index"
            :label="item.v"
            :value="item.k"
          ></el-option>
        </el-select>
      </el-form-item>
      <!-- 渠道版本 -->
      <dtu-version-list
        v-if="allow_update == 0"
        @emitData="emitDtuVersionListData"
        :dtuVersionData="dtuVersionData"
        :isEdit="queryId"
      ></dtu-version-list>

      <dtu-version-list-v2 ref="dtuVersionList" @emitData="emitAppData" :appId="appId" v-else></dtu-version-list-v2>

      <!--生效人群-->
      <userType :formData="formData" :config="config"></userType>

      <el-form-item label="开放流量类型:" prop="rateType">
        <el-radio-group v-model="formData.rateType">
          <el-radio
            v-for="item in config.rateType"
            :key="item.k"
            :label="item.k"
            :value="item.k"
          >{{item.v}}</el-radio>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="开发流量:" prop="rate" v-if="formData.rateType == 1">
        <el-input type="number" v-model="formData.rate" class="rate-input" placeholder="请输入数值"></el-input>%
      </el-form-item>
      <el-form-item label="ab配置:" v-if="formData.rateType == 2">
        <AbTest :rateAb="rateAb" @emitRateAb="emitRateAb" />
      </el-form-item>

      <el-form-item label="有效时间:" prop="rangeDate">
        <el-date-picker
          v-model="formData.rangeDate"
          value-format="yyyy-MM-dd HH:mm:ss"
          type="datetimerange"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
        ></el-date-picker>
      </el-form-item>
      <el-form-item prop="sort" label="排序:">
        <el-input v-model="formData.sort" style="width: 100px;" type="number"></el-input>越小越在前面
      </el-form-item>

      <el-form-item label="是否有效:" prop="status">
        <el-select v-model="formData.status">
          <el-option v-for="item in config.isok" :key="item.k" :label="item.v" :value="item.k"></el-option>
        </el-select>
      </el-form-item>

      <el-form-item>
        <el-button @click="cancel">取消</el-button>
        <el-button type="primary" v-if="allow_update == 1" @click="onSubmit()">保存</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script src="./add.js"></script>

```

```js
/* eslint-disable padded-blocks */
/* eslint-disable eol-last */
/* eslint-disable indent */
import QuRequest from '@/core/QuRequest';
import ComModChanDialog from './ComModChanDialog';

const quRequest = new QuRequest();

const processData = function(item) {
    const { size, sizeItem } = item;
    if (!size) {
        item.size = '';
    } else {
        item.size = size
            .split(',')
            .filter(v => {
                return sizeItem.some(x => {
                    if (x.k === v) {
                        return true;
                    }
                });
            })
            .filter(x => !!x)
            .join(',');
    }
    item.position = item.position || '';
    item.positionItem = item.positionItem || [];
};

export default {
    name: 'commercial-modify',
    data() {
        return {
            tableData: [],
            changeTarget: {
                appDisplay: '',
                advertisementName: '',
                // advertisementId: '',
                channel: '',
                begin: '',
                isShow: 2,
                hideDay: 0,
                adsType: 0,
                adsEmbedType: 1
            },
            checkList: [],
            popupAds: [],
            changeLoading: false,
            locationId: '', // 新增售卖方式字段.
            app: '',
            videoCfg: '', // 后台判断是否显示激励视频配置
            // e-radio-group 不支持二级对象更新.
            showPosition: '', // 显示位置.
            showLocation: [], // 显示位置.

            dialog: {
                aVisible: false,
                disabled: true,
                title: '设置',
                dayTotal: null,
                peopleDayTotal: null,
                id: null
            },
            sizeDialog: {
                show: false,
                value: [],
                items: []
            },
            posDialog: {
                show: false,
                value: [],
                items: []
            },
            // 章节末视频激励广告位.
            policy_cfg_value: {
                askType: '',
                askTime: 1,
                exemptAdTime: '', // 免除广告时间，单位：秒.
                dailyFrequency: '', // 每日展现频率.
                ruleDesc: '', // 规则说明.
                exemptAdTimeBtnDisplay: '',
                vipText: '',
                spaceChapterNum: '', // 章节头激励视频广告位-间隔章节数
                mustWatchTimes: 0, // 章节头激励视频广告位-强制观看秒数
                landingUrl: '', // 会员中心入口策略 - 落地链接
                adsEmbedTypeLine: '', // 内嵌广告策略插入行：默认4行
                redirectType: 2, // 跳转类型
                redirectTarget: '', // 跳转URL
                redirectBtn: '' // 跳转文案
            },
            vipPositionCfg: [],
            vipPosition: '',
            config: {
                urlto: [{ k: 2, v: 'H5' }, { k: 5, v: 'cpc sdk H5' }]
            }
        };
    },
    computed: {
        isMultiSize() {
            return true;
        },
        needPosition() {
            return this.locationId === 'ADCodeReadInsert'; // && (this.app === '2' || this.app === '3')
        }
    },
    mounted() {

        quRequest
            .send(quRequest.Apis.qukan.commercialization.getInfo, {
                id: this.$route.query.id,
                appType: 2

            })
            .then(res => {
                // 预处理size，移除不应该存在的旧数据
                res.data.ids.forEach(processData);
                console.log(res.data)
                this.changeTarget = res.data;
                this.changeTarget.isShow = +this.changeTarget.isShow;
                this.changeTarget.adsType = +this.changeTarget.adsType;
                this.changeTarget.adsEmbedType = +this.changeTarget.adsEmbedType;
                this.tableData = res.data.ids;
                this.popupAds = res.data.popupAds;
                this.app = res.data.app;
                this.locationId = res.data.locationId;
                // this.locationId = 'ADCodeReadInsert'
                this.videoCfg = res.data.videoCfg;
                this.showPosition = +res.data.show_position;
                console.log(this.locationId)
                res.data.policy_cfg &&
                    res.data.policy_cfg.showLocation &&
                    (this.showLocation = res.data.policy_cfg.showLocation || '');
                this.vipPositionCfg =
                    (res.data.policy_cfg && res.data.policy_cfg.vipPosition) || [];
                // 章节末视频激励广告位.
                if (res.data.policy_cfg_value) {
                    /* eslint-disable-next-line */
                    const policy_cfg_value = res.data.policy_cfg_value;
                    this.policy_cfg_value = {
                        exemptAdTime: policy_cfg_value.exemptAdTime,
                        askType: policy_cfg_value.askType,
                        dailyFrequency: policy_cfg_value.dailyFrequency,
                        ruleDesc: policy_cfg_value.ruleDesc,
                        exemptAdTimeBtnDisplay: policy_cfg_value.exemptAdTimeBtnDisplay,
                        vipText: policy_cfg_value.vipText,
                        landingUrl: policy_cfg_value.landingUrl,
                        mustWatchTimes: policy_cfg_value.mustWatchTimes,
                        spaceChapterNum: policy_cfg_value.spaceChapterNum,
                        adsEmbedTypeLine: policy_cfg_value.adsEmbedTypeLine,
                        askTime: policy_cfg_value.askTime,
                        redirectType: policy_cfg_value.redirectType,
                        redirectTarget: policy_cfg_value.redirectTarget,
                        redirectBtn: policy_cfg_value.redirectBtn,
                        floatChapterNum: policy_cfg_value.floatChapterNum,
                        everyFloatChapterNum: policy_cfg_value.everyFloatChapterNum,
                        floatSeconds: policy_cfg_value.floatSeconds
                    };
                    this.vipPosition = policy_cfg_value.vipPosition;
                }
            })
            .catch(err => {
                this.$message.error(`获取失败: ${err.message}`);
                throw err;
            });
    },
    components: {
        ComModChanDialog
    },
    methods: {
        // 增加ads
        addADs() {
            this.$refs.channelDialog.showDialog();
        },
        closeChDialog(data) {
            if (data) {
                this.tableData.push(...data);
            }
        },
        // 删除指定item
        delData(scope) {
            this.$confirm('确认删除吗？')
                // eslint-disable-next-line no-unused-vars
                .then(_ => {
                    const { $index } = scope;
                    this.tableData.splice($index, 1);
                })
                // eslint-disable-next-line no-unused-vars
                .catch(_ => {});
        },
        getPosStr({ position, positionItem }) {
            const rtn = position
                .split(',')
                .map(v => {
                    let str = '';
                    positionItem.some(x => {
                        if (x.k === v) {
                            str = x.v;
                            return true;
                        }
                    });
                    return str;
                })
                .filter(x => !!x)
                .join(',');
            return rtn || '设置';
        },
        toSetPos(item) {
            const { posDialog } = this;
            posDialog.value = item.position.split(',').filter(k => {
                return item.positionItem.some(pos => pos.k === k);
            });
            posDialog.items = item.positionItem;
            posDialog.show = true;
            posDialog.item = item;
        },
        onSetPos() {
            const { posDialog } = this;
            const { item } = posDialog;
            if (posDialog.value.length === 0 && item.status === 1) {
                this.$message.error('请选择位置');
                return;
            }
            posDialog.item.position = posDialog.value.join(',');
            posDialog.show = false;
        },
        getSizeStr({ size, sizeItem }) {
            const rtn = size
                .split(',')
                .map(v => {
                    let str = '';
                    sizeItem.some(x => {
                        if (x.k === v) {
                            str = x.v;
                            return true;
                        }
                    });
                    return str;
                })
                .filter(x => !!x)
                .join(',');
            return rtn || '设置';
        },
        toSetSize(item) {
            const { sizeDialog } = this;
            const { size, sizeItem } = item;
            sizeDialog.value = size ? size.split(',') : [];
            sizeDialog.items = sizeItem;
            sizeDialog.show = true;
            sizeDialog.item = item;
        },
        onSetSize() {
            const { sizeDialog } = this;
            const { item } = sizeDialog;
            if (sizeDialog.value.length === 0 && item.status === 1) {
                this.$message.error('请选择比例');
                return;
            }
            sizeDialog.item.size = sizeDialog.value.join(',');
            sizeDialog.show = false;
        },
        indexMethod(index) {
            return index + 1;
        },
        // 启动状态.
        changeStatus(scope) {
            if (scope.row.showNum === 0) {
                scope.row.defaultChannel = 0;
                scope.row.status = 0;
            }
            if (scope.row.status === 0) {
                scope.row.defaultChannel = 0;
            }
        },
        // 打底开启.
        changeDefaultChannel(scope) {
            const data = this.tableData;
            // 遍历数组 保证打底的只有一个.
            for (let i = 0; i < data.length; i++) {
                if (
                    parseInt(data[i].defaultChannel) === 1 &&
                    data[i].id !== scope.row.id
                ) {
                    data[i].defaultChannel = 0;
                }
            }
        },

        // 信息提交.
        // eslint-disable-next-line no-unused-vars
        saveInfoRes(e) {
            this.changeLoading = true;
            // eslint-disable-next-line
            const {
                changeTarget,
                policy_cfg_value,
                tableData,
                popupAds,
                showPosition
            } = this;
            const policyCfgValue = {
                askType: policy_cfg_value.askType,
                exemptAdTime: policy_cfg_value.exemptAdTime || '', // 免除广告时间.
                dailyFrequency: policy_cfg_value.dailyFrequency || '', // 每日出现频率.
                ruleDesc: policy_cfg_value.ruleDesc || '', // 规则说明.
                exemptAdTimeBtnDisplay: policy_cfg_value.exemptAdTimeBtnDisplay || '',
                vipText: policy_cfg_value.vipText,
                landingUrl: policy_cfg_value.landingUrl,
                mustWatchTimes: policy_cfg_value.mustWatchTimes,
                spaceChapterNum: policy_cfg_value.spaceChapterNum,
                vipPosition: this.vipPosition,
                adsEmbedTypeLine: policy_cfg_value.adsEmbedTypeLine,
                askTime: policy_cfg_value.askTime,
                redirectType: policy_cfg_value.redirectType,
                redirectTarget: policy_cfg_value.redirectTarget,
                redirectBtn: policy_cfg_value.redirectBtn,
                floatChapterNum: policy_cfg_value.floatChapterNum,
                everyFloatChapterNum: policy_cfg_value.everyFloatChapterNum,
                floatSeconds: policy_cfg_value.floatSeconds
            };
            if (!this.vipPositionCfg.length) {
                delete policyCfgValue.vipPosition;
            }
            quRequest
                .send(quRequest.Apis.qukan.commercialization.saveInfo, {
                    // advertisementId: changeTarget.advertisementId || 0,
                    id: this.$route.query.id,
                    appType: 2,
                    frequency: changeTarget.frequency,
                    isShow: changeTarget.isShow,
                    hideDay: changeTarget.hideDay,
                    channel: changeTarget.channel,
                    begin: changeTarget.begin,
                    adsType: changeTarget.adsType,
                    ids: JSON.stringify(tableData),
                    popupAds: JSON.stringify(popupAds),
                    show_position: showPosition || 1,
                    // 章节末视频激励广告位 参数.
                    policy_cfg_value: JSON.stringify(policyCfgValue),
                    adsEmbedType: changeTarget.adsEmbedType
                })
                .then(r => {
                    this.changeLoading = false;
                    this.$message({
                        showClose: true,
                        message: r.data.clear,
                        type: 'success'
                    });
                    this.back();
                })
                .catch(err => {
                    this.changeLoading = false;
                    this.$message.error(`获取失败: ${err.message}`);
                    throw err;
                });
        },
        // 信息提交校验.
        submitInfo(e) {
            let toastText = '';

            // 广告标识id.
            if (this.changeTarget.channel === '') {
                return this.$message.warning('广告标识id 未获取到～');
            }

            // 广告渠道设置.
            const { tableData, isMultiSize, needPosition } = this;
            let len = 0;
            tableData.some(item => {
                if (isMultiSize) {
                    if (!item.size && item.status === '1') {
                        return (toastText = '广告策略 | 请选择比例～');
                    }
                } else {
                    if (!item.size && item.status === '1') {
                        return (toastText = '广告策略 | 请选择比例～');
                    }
                }
                if (needPosition && item.status === '1' && !item.position) {
                    return (toastText = '广告策略 | 请选择位置～');
                }
                if (item.status === '1' && (!item.showNum || isNaN(item.showNum))) {
                    return (toastText = '广告渠道设置 | 展示次数填写有误～');
                }
                if (item.status === '1' && !item.id) {
                    return (toastText = '广告渠道设置 | 启动状态未开启～');
                }

                if (item.defaultChannel === '1') {
                    len++;
                }
            });

            if (toastText !== '') {
                return this.$message.warning(toastText);
            }

            if (len === 0) {
                return this.$message.warning('广告渠道设置 | 打底未开启～');
            }

            // 广告策略.
            const popupAds = this.popupAds;
            popupAds.some(item => {
                if (item.status && !item.rate) {
                    return (toastText = '广告策略 | 请输入触发概率 勾选有误～');
                }
                if (item.status && (!item.maxShow || isNaN(item.maxShow))) {
                    return (toastText = '广告策略 | 每天每个用户弹出次 填写有误～');
                }

                if (
                    item.rate &&
                    (item.rate > 100 || item.rate < 0 || isNaN(item.rate))
                ) {
                    return (toastText = '广告策略 | 请输入触发概率 填写错误～');
                }
            });

            if (toastText !== '') {
                return this.$message.warning(toastText);
            }

            // 广告位优化 | v2.8.0.
            if (
                this.locationId === 'ADCodeBookBanner' ||
                this.locationId === 'ADCodeBookShelfInsert' ||
                this.locationId === 'ADCodeMySelfInsert'
            ) {
                if (this.showPosition === '') {
                    // 显示位置.
                    return this.$message.warning('广告位优化-显示位置 | 未填写～');
                } else if (
                    parseInt(this.showPosition) < 0 ||
                    parseInt(this.showPosition) > 100
                ) {
                    return this.$message.warning(
                        '广告位优化-显示位置 | 填写最大为100，最小为0～'
                    );
                } else if (isNaN(this.showPosition)) {
                    return this.$message.warning('广告位优化-显示位置 | 填写要求数字～');
                }
            } else if (
                this.locationId !== 'ADCodeBookBanner' &&
                this.locationId !== 'ADCodeBookShelfInsert' &&
                this.locationId !== 'ADCodeMySelfInsert' &&
                this.locationId !== 'ADCodeReadEndIncentiveVideo'
            ) {
                if (
                    this.changeTarget.hideDay.toString() === '' ||
                    this.policy_cfg_value.adsEmbedTypeLine === '' ||
                    this.changeTarget.begin.toString() === '' ||
                    isNaN(this.changeTarget.begin)
                ) {
                    return this.$message.warning('插入行 | 周期 | 章节 字段未填写～');
                } else if (this.changeTarget.frequency === '') {
                    // 频率.
                    return this.$message.warning('广告位优化-频率 | 未填写～');
                } else if (isNaN(this.changeTarget.frequency)) {
                    return this.$message.warning('广告位优化-频率 | 填写要求数字～');
                } else if (
                    parseInt(this.changeTarget.frequency) < 0 ||
                    parseInt(this.changeTarget.frequency) > 100
                ) {
                    return this.$message.warning(
                        '广告位优化-频率 | 填写最大为100，最小为0～'
                    );
                }
            }

            if (this.locationId === 'ADCodeReadEndIncentiveVideo') {
                if (
                    this.policy_cfg_value.exemptAdTime === '' ||
                    isNaN(this.policy_cfg_value.exemptAdTime)
                ) {
                    return this.$message.warning('免除广告的时间输入不对～');
                }
                if (
                    this.policy_cfg_value.dailyFrequency === '' ||
                    isNaN(this.policy_cfg_value.dailyFrequency)
                ) {
                    return this.$message.warning('每日出现频率输入不对～');
                }
                if (this.policy_cfg_value.ruleDesc === '') {
                    return this.$message.warning('规则说明还是要写的～');
                }
            }
            // 底部悬浮广告 | 频率输入范围 1-100.
            if (
                this.locationId === 'ADCodeReadBottomFloat' &&
                parseInt(this.changeTarget.frequency) <= 0
            ) {
                return this.$message.warning(
                    '广告位优化-频率(底部悬浮广告) | 频率输入范围 1-100.'
                );
            }

            // 会员中心落地链接
            const regUrl = /[a-zA-z]+:\/\/[^\s]*/;
            if (
                this.locationId === 'ADCodeReadStartIncentiveVideo' &&
                !regUrl.test(this.policy_cfg_value.landingUrl)
            ) {
                return this.$message.warning('请填写会员中心落地链接');
            }

            // 保存接口请求.
            this.saveInfoRes(e);
        },
        back() {
            this.$router.go(-1)
                // this.$router.push({
                //     path: 'commercial',
                //     query: {
                //         way: 'back'
                //     }
                // });
        },
        // 显示弹窗.
        openDialog(title, data, id, index) {
            this.dialog = {
                title: title,
                aVisible: true,
                dayTotal: data.dailyShowNum,
                peopleDayTotal: data.dailyUserShowNum,
                id: id,
                index
            };
        },
        // 关闭弹窗.
        hideDialog() {
            this.dialog.aVisible = false;
        },
        // 设置投放总量.
        sumbitSetInfoHandle() {
            const data = this.tableData;
            for (let i = 0; i < data.length; i++) {
                if (data[i].id === this.dialog.id && i === this.dialog.index) {
                    data[i].putAmount.dailyShowNum = this.dialog.dayTotal;
                    data[i].putAmount.dailyUserShowNum = this.dialog.peopleDayTotal;
                }
            }
            this.dialog.aVisible = false;
        }
    }
}
```

