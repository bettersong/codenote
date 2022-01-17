```js
gotoComment(e) {
      let timer;
      if (timer) {
        clearTimeout(timer);
        timer = null;
      }
      this.delta = new Date().getTime() - this.now; // 两次点击时间差
      this.now = new Date().getTime();
      if (this.delta > 0 && this.delta <= 250) {
        console.log("双击了");
      } else {
        timer = setTimeout(() => {
          if (!this.delta || this.delta > 200) {
            console.log(`[${e.target.tagName}单击]`);
            localStorage.setItem("pageTop", window.scrollY);
            this.$emit("toEdit", this.comItem.comment_id);
          }
        }, 400);
        //单击
      }
      // this.timer = setTimeout(() => {
      //   localStorage.setItem("pageTop", window.scrollY);
      //   this.$emit("toEdit", this.comItem.comment_id);
      // }, 300);
    },
```

```js

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

