<template>
    <view>
        <!-- 顶部内容 -->
        <view class="top-content" :style="top_content_style">
            <!-- 内容 -->
            <view class="content padding-horizontal-main">
                <view class="head-base pr oh">
                    <!-- 左侧头像 -->
                    <view class="padding-bottom-xxl oh tc fl">
                        <image @tap="preview_event" @error="user_avatar_error" class="head-avatar circle bg-white va-m" :src="avatar" mode="widthFix"></image>
                        <view class="va-m dis-inline-block cr-white margin-left-lg tl">
                            <view data-value="/pages/personal/personal" @tap="url_event">
                                <view>
                                    <text class="va-m">{{nickname}}</text>
                                    <view class="dis-inline-block va-m margin-left-sm">
                                        <uni-icons type="compose" size="14" color="#fff"></uni-icons>
                                    </view>
                                </view>
                                <view v-if="(user_id || null) != null" class="br round tc padding-left-lg padding-right-lg margin-top-xs dis-inline-block">ID {{user_id}}</view>
                            </view>
                        </view>
                    </view>

                    <!-- 右上角 -->
                    <view class="head-right pa">
                        <view v-if="(qrcode_page_url || null) != null" class="item pr dis-inline-block" :data-value="qrcode_page_url" @tap="url_event">
                            <image class="icon" :src="common_static_url+'qrcode-icon.png'" mode="widthFix"></image>
                        </view>
                        <view class="item pr dis-inline-block margin-left-xxl" data-value="/pages/setup/setup" @tap="url_event">
                            <image class="icon" :src="common_static_url+'gear-icon.png'" mode="widthFix"></image>
                        </view>
                        <view class="item pr dis-inline-block margin-left-xxl" data-value="/pages/message/message" @tap="url_event">
                            <image class="icon" :src="common_static_url+'notification-icon.png'" mode="widthFix"></image>
                            <view class="badge-icon pa">
                                <component-badge :propNumber="message_total"></component-badge>
                            </view>
                        </view>
                    </view>
                </view>

                <!-- 副导航 -->
                <view v-if="common_app_is_head_vice_nav == 1 && head_nav_list.length > 0" class="head-nav oh bg-white padding-main border-radius-main spacing-mb">
                    <block v-for="(item, index) in head_nav_list" :key="index">
                        <navigator :url="'/pages/' + item.url + '/' + item.url" hover-class="none">
                            <view class="head-nav-item tc fl">
                                <view class="nav-value fw-b text-size">{{item.count}}</view>
                                <view class="nav-name cr-gray">{{item.name}}</view>
                            </view>
                        </navigator>
                    </block>
                </view>
                
                <!-- 订单导航 -->
                <view v-if="(navigation_order || null) != null" class="nav-list bg-white bg-white padding-main border-radius-main spacing-mb">
                    <!-- 主导航 -->
                    <view :data-value="navigation_order.event_value" :data-type="navigation_order.event_type" @tap="navigation_event" class="nav-item br-b cp">
                        <view class="arrow-right">
                            <image :src="navigation_order.images_url" class="item-icon va-m" mode="widthFix"></image>
                            <text class="item-name va-m cr-base margin-left-sm">{{navigation_order.name}}</text>
                            <text v-if="(navigation_order.desc || null) != null" class="item-desc fr tr single-text cr-grey">{{navigation_order.desc}}</text>
                        </view>
                    </view>
                    <!-- 订单自定义副导航 -->
                    <view class="nav-list-sub oh margin-top-sm">
                        <block v-for="(items, index) in user_order_status_list" :key="index">
                            <navigator :url="items.url" hover-class="none">
                                <view class="item pr fl tc">
                                    <view class="badge-icon pa">
                                        <component-badge :propNumber="items.count"></component-badge>
                                    </view>
                                    <image class="item-icon margin-bottom-xs" :src="static_url+'order-icon-' + items.status + '.png'" mode="aspectFill"></image>
                                    <view class="item-name cr-base">{{items.name}}</view>
                                </view>
                            </navigator>
                        </block>
                    </view>
                </view>
                
                <!-- 聚合导航 -->
                <view class="nav-box padding-main border-radius-main bg-white spacing-mb">
                    <view class="br-b padding-main item-title fw-b text-size">我的服务</view>
                    <view class="oh margin-top-sm">
                        <block v-for="(item, index) in navigation" :key="index">
                            <!-- 这里不展示订单导航 -->
                            <block v-if="item.event_value != '/pages/user-order/user-order'">
                                <view :data-value="item.event_value" :data-type="item.event_type" @tap="navigation_event" class="nav-item padding-main fl tc cp">
                                    <image :src="item.images_url" class="item-icon" mode="widthFix"></image>
                                    <view class="item-name single-text cr-base">{{item.name}}</view>
                                </view>
                            </block>
                        </block>
            
                        <!-- 清除缓存 -->
                        <view class="nav-item padding-main fl tc cp" @tap="remove_user_cache_event">
                            <image :src="common_static_url+'cache-icon.png'" class="item-icon" mode="widthFix"></image>
                            <view class="item-name single-text cr-base">清除缓存</view>
                        </view>
                
                        <!-- 联系客服 -->
                        <view v-if="(common_app_customer_service_tel || null) != null" class="nav-item padding-main fl tc cp" @tap="call_event">
                            <image :src="common_static_url+'customer-service-icon.png'" class="item-icon" mode="widthFix"></image>
                            <view class="item-name single-text cr-base">联系客服</view>
                        </view>
                    </view>
                </view>
            </view>
        </view>

        <!-- 用户中心公告 -->
        <view v-if="(common_user_center_notice || null) != null" class="padding-horizontal-main padding-bottom-main">
            <view class="notice-content">{{common_user_center_notice}}</view>
        </view>

        <!-- 在线客服 -->
        <component-online-service :propIsNav="true" :propIsBar="true"></component-online-service>

        <!-- 快捷导航 -->
        <component-quick-nav :propIsNav="true" :propIsBar="true"></component-quick-nav>

        <!-- 版权信息 -->
        <component-copyright></component-copyright>
    </view>
</template>

<script>
    const app = getApp();
    import componentQuickNav from "../../components/quick-nav/quick-nav";
    import componentBadge from "../../components/badge/badge";
    import componentCopyright from "../../components/copyright/copyright";
    import componentOnlineService from "../../components/online-service/online-service";

    var common_static_url = app.globalData.get_static_url('common');
    var static_url = app.globalData.get_static_url('user');
    export default {
        data() {
            return {
                common_static_url: common_static_url,
                static_url: static_url,
                avatar: app.globalData.data.default_user_head_src,
                user_id: '',
                nickname: "用户名",
                message_total: 0,
                head_nav_list: [
                    { name: "订单总数", url: "user-order", count: 0 },
                    { name: "商品收藏", url: "user-favor", count: 0 },
                    { name: "我的足迹", url: "user-goods-browse", count: 0 },
                    { name: "我的积分", url: "user-integral", count: 0 },
                ],
                user_order_status_list: [
                    { name: "待付款", status: 1, count: 0, url: "/pages/user-order/user-order?status=1" },
                    { name: "待发货", status: 2, count: 0, url: "/pages/user-order/user-order?status=2" },
                    { name: "待收货", status: 3, count: 0, url: "/pages/user-order/user-order?status=3" },
                    { name: "已完成", status: 4, count: 0, url: "/pages/user-order/user-order?status=4" },
                    { name: "退款/售后", status: 101, count: 0, url: "/pages/user-orderaftersale/user-orderaftersale" },
                ],
                navigation_order: null,
                // 远程自定义导航
                navigation: [],
                // 基础配置
                common_app_customer_service_tel: null,
                common_user_center_notice: null,
                common_app_is_online_service: 0,
                common_app_is_head_vice_nav: 0,
                // 顶部样式配置
                top_content_style: 'background-image: url("'+static_url+'nav-top.png");'+'padding-top:'+(parseInt(app.globalData.get_system_info('statusBarHeight', 0))+5)+'px;',
                // 付款码地址
                qrcode_page_url: null
            };
        },

        components: {
            componentQuickNav,
            componentBadge,
            componentCopyright,
            componentOnlineService
        },
        props: {},

        onShow() {
            // 数据加载
            this.init();

            // 初始化配置
            this.init_config();
        },

        // 下拉刷新
        onPullDownRefresh(e) {
            this.init();
        },

        methods: {
            // 初始化配置
            init_config(status) {
                if ((status || false) == true) {
                    // 会员码及付款码入口
                    var qrcode_page_url = null;
                    if(app.globalData.get_config('plugins_base.wallet', null) != null) {
                        qrcode_page_url = '/pages/plugins/wallet/payment-code/payment-code';
                    }
                    if(app.globalData.get_config('plugins_base.membershiplevelvip', null) != null) {
                        qrcode_page_url = '/pages/plugins/membershiplevelvip/member-code/member-code';
                    }
                    this.setData({
                        common_app_customer_service_tel: app.globalData.get_config('config.common_app_customer_service_tel'),
                        common_user_center_notice: app.globalData.get_config('config.common_user_center_notice'),
                        common_app_is_online_service: app.globalData.get_config('config.common_app_is_online_service'),
                        common_app_is_head_vice_nav: app.globalData.get_config('config.common_app_is_head_vice_nav'),
                        qrcode_page_url: qrcode_page_url
                    });
                } else {
                    app.globalData.is_config(this, 'init_config');
                }
            },

            // 获取数据
            init(e) {
                var user = app.globalData.get_user_info(this, "init");
                if (user != false) {
                    // 用户未绑定用户则转到登录页面
                    if (app.globalData.user_is_need_login(user)) {
                        uni.stopPullDownRefresh();
                        uni.showModal({
                            title: '温馨提示',
                            content: '绑定手机号码',
                            confirmText: '确认',
                            cancelText: '暂不',
                            success: result => {
                                uni.stopPullDownRefresh();
                                if (result.confirm) {
                                    uni.navigateTo({
                                        url: "/pages/login/login?event_callback=init"
                                    });
                                }
                                this.set_user_base(user);
                            }
                        });

                        // 分享菜单处理
                        app.globalData.page_share_handle();
                    } else {
                        this.set_user_base(user);
                        this.get_data();
                    }
                } else {
                    uni.stopPullDownRefresh();

                    // 分享菜单处理
                    app.globalData.page_share_handle();
                }
            },
            
            // 设置用户基础信息
            set_user_base(user) {
                if((user.id || null) != null) {
                    this.setData({user_id: user.id});
                }
                if((user.avatar || null) != null) {
                    this.setData({avatar: user.avatar});
                }
                if((user.user_name_view || null) != null) {
                    this.setData({nickname: user.user_name_view});
                }
            },

            // 获取数据
            get_data() {
                uni.request({
                    url: app.globalData.get_request_url("center", "user"),
                    method: 'POST',
                    data: {},
                    dataType: 'json',
                    success: res => {
                        uni.stopPullDownRefresh();
                        if (res.data.code == 0) {
                            var data = res.data.data;
                            // 订单状态
                            var temp_user_order_status_list = this.user_order_status_list;
                            if ((data.user_order_status || null) != null && data.user_order_status.length > 0) {
                                for (var i in temp_user_order_status_list) {
                                    for (var t in data.user_order_status) {
                                        if (temp_user_order_status_list[i]['status'] == data.user_order_status[t]['status']) {
                                            temp_user_order_status_list[i]['count'] = data.user_order_status[t]['count'];
                                            break;
                                        }
                                    }
                                }
                            }

                            // 头部导航总数
                            var temp_head_nav_list = this.head_nav_list;
                            temp_head_nav_list[0]['count'] = (data.user_order_count || 0) == 0 ? 0 : data.user_order_count;
                            temp_head_nav_list[1]['count'] = (data.user_goods_favor_count || 0) == 0 ? 0 : data.user_goods_favor_count;
                            temp_head_nav_list[2]['count'] = (data.user_goods_browse_count || 0) == 0 ? 0 : data.user_goods_browse_count;
                            temp_head_nav_list[3]['count'] = (data.integral || 0) == 0 ? 0 : data.integral;

                            // 数据设置
                            var upd_data = {
                                user_order_status_list: temp_user_order_status_list,
                                message_total: parseInt(data.message_total || 0),
                                head_nav_list: temp_head_nav_list,
                                navigation: data.navigation || []
                            }
                            // 是否存在配置订单导航
                            var temp_nav_order = null;
                            if(upd_data.navigation.length > 0) {
                                for(var i in upd_data.navigation) {
                                    var url = app.globalData.get_url_main_part(upd_data.navigation[i]['event_value']);
                                    if(url == '/pages/user-order/user-order') {
                                        temp_nav_order = upd_data.navigation[i];
                                        upd_data.navigation.splice(i, 1);
                                        break;
                                    }
                                }
                            }
                            upd_data['navigation_order'] = temp_nav_order;

                            // 用户基础信息处理
                            if((data.avatar || null) != null) {
                                upd_data['avatar'] = data.avatar;
                            }
                            if((data.user_name_view || null) != null) {
                                upd_data['nickname'] = data.user_name_view;
                            }
                            this.setData(upd_data);

                            // 导航购物车处理
                            var cart_total = parseInt(data.cart_total.buy_number || 0);
                            if (cart_total <= 0) {
                                app.globalData.set_tab_bar_badge(2, 0);
                            } else {
                                app.globalData.set_tab_bar_badge(2, 1, cart_total);
                            }
                        } else {
                            if (app.globalData.is_login_check(res.data, this, 'get_data')) {
                                app.globalData.showToast(res.data.msg);
                            }
                        }
                        
                        // 分享菜单处理、延时执行，确保基础数据已加载完成
                        setTimeout(function() {
                            app.globalData.page_share_handle();
                        }, 3000);
                    },
                    fail: () => {
                        uni.stopPullDownRefresh();
                        app.globalData.showToast('服务器请求出错');
                    }
                });
            },

            // 清除缓存
            remove_user_cache_event(e) {
                app.globalData.remove_user_cache_event();
            },

            // 客服电话
            call_event() {
                if (this.common_app_customer_service_tel == null) {
                    app.globalData.showToast('客服电话有误');
                } else {
                    app.globalData.call_tel(this.common_app_customer_service_tel);
                }
            },

            // 头像查看
            preview_event() {
                if (app.globalData.data.default_user_head_src != this.avatar) {
                    uni.previewImage({
                        current: this.avatar,
                        urls: [this.avatar]
                    });
                }
            },

            // 头像加载错误
            user_avatar_error(e) {
                this.setData({
                    avatar: app.globalData.data.default_user_head_src
                });
            },

            // 远程自定义导航事件
            navigation_event(e) {
                app.globalData.operation_event(e);
            },

            // url事件
            url_event(e) {
                app.globalData.url_event(e);
            }
        }
    };
</script>
<style>
    @import './user.css';
</style>