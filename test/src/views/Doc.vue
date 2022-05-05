<template>
    <div style="text-align:center;background:#FAFAFA;min-width:1900px">
    <div class="subNavi">
      <a-row>
        <a-col :span="15">
          <div style="margin:8px 0 0 128px;text-align:left">
            <a-icon @click="toWorkshop" type="arrow-left" class="returnNarrow" />
            <a-popover placement="bottom">
              <template slot="content">点击即可收藏文档哦~</template>
              <a-rate
                @change="judgeFav"
                :count="1"
                :value="isFav"
                style="margin:0px 0 5px 5px;font-size:24px;"
              />
              <!--↑说起来你可能不信，但是这个可以当按钮来用-->
            </a-popover>
            <!--span style="font-size:24px;margin-right:5px">{{title}}</span-->
            <span style="font-size:24px;margin-right:5px">
              <b>
                <a-input
                  v-model="title"
                  size="large"
                  style="border:0;width:200px;background:#FAFAFA;font-size:18px"
                />
              </b>
            </span>
            <span style="margin-left:5px">上次修改时间是{{lastetidtimeString}}，共计修改了{{editcount}}次。</span>
          </div>
        </a-col>
        <a-col :span="3"></a-col>
        <a-col :span="6">
          <div style="margin:11px 48px 0 0;text-align:right">
            <a-popover placement="bottomLeft" trigger="click">
              <template slot="content" style="text-align:right">
                <div style="margin-top:12px">团队权限设置</div>
                <div style="margin-top:5px">
                  <a-radio-group
                    style="margin-bottom:12px"
                    v-model="tempteamauth"
                    @change="onTeamAuthChange"
                  >
                    <a-radio-button :value="1">仅查看</a-radio-button>
                    <a-radio-button :value="3">可讨论</a-radio-button>
                    <a-radio-button :value="7">可编辑</a-radio-button>
                  </a-radio-group>
                </div>
              </template>
              <a-button style="margin-right:24px" v-if="teamid!=0&&isteamauth">
                <a-icon type="safety" />团队权限
              </a-button>
            </a-popover>
            <a-popover placement="bottomLeft" trigger="click">
              <template slot="content" style="text-align:center">
                <div style="margin-top:12px" v-if="creatorid==$store.state.userid">权限设置</div>
                <div style="margin-top:5px;text-align:center" v-if="creatorid==$store.state.userid">
                  <a-radio-group v-model="tempauth" @change="onAuthChange">
                    <a-radio-button :value="1">仅查看</a-radio-button>
                    <a-radio-button :value="3">可讨论</a-radio-button>
                    <a-radio-button :value="7">可编辑</a-radio-button>
                  </a-radio-group>
                </div>
                <div style="margin-top:12px" v-if="tempauth!=0">
                  <div>分享链接</div>
                  <a-input-search
                    :value="pagePath"
                    @search="onShareCopy"
                    style="margin-top:5px;width:220px"
                    disabled
                  >
                    <a-button
                      class="tag-read"
                      v-clipboard:copy="pagePath"
                      v-clipboard:success="onShareCopy"
                      v-clipboard:error="onShareCopyError"
                      @click="onShareCopy"
                      slot="enterButton"
                    >复制</a-button>
                  </a-input-search>
                </div>
                <div v-if="tempauth!=0" style="text-align:center;margin:12px;">
                  <qrcode :value="pagePath" :options="{ size: 120 }"></qrcode>
                </div>
                <div style="text-align:center;margin-top:10px;margin-bottom:10px" v-if="tempauth==0">文档未公开</div>
                <div style="text-align:center">
                  <a-button
                    style="margin:0px 10px 10px 0px"
                    @click="stopShare"
                    v-if="creatorid==$store.state.userid"
                  >停止分享</a-button>
                </div>
              </template>
              <a-button>
                <a-icon type="share-alt" />分享
              </a-button>
            </a-popover>
            <!--a-button @click="test" ><a-icon type="save"/>TEST_测试</a-button-->
          </div>
        </a-col>
      </a-row>
    </div>

    <div style="text-align:center">
      <a-row>
        <a-col :span="5">
          <a-timeline style="width:260px;margin:64px">
            <a-timeline-item
              v-for="item in editrecord"
              :key="item.edittime"
            >{{item.userid}}于{{item.edittimestring}}编辑</a-timeline-item>
          </a-timeline>
        </a-col>
        <a-col :span="14">
          <div class="text-editor">
            <mavon-editor
              v-bind:editable="iseditable"
              :subfield="iseditable"
              :toolbarsFlag="iseditable"
              defaultOpen="preview"
              v-model="content"
              ref="md"
              @imgAdd="$imgAdd"
              @imgDel="$imgDel"
              @save="saveDoc"
              @change="textChange"
              style="min-height:1000px;z-index:0"
            />
          </div>
        </a-col>
        <a-col :span="5">
          <div>
            <div style="margin:48px">
              <a-list
                class="commentList"
                :header="`${data.length}个评论`"
                item-layout="horizontal"
                :data-source="data"
              >
                <a-list-item slot="renderItem" slot-scope="item">
                  <a-comment>
                    <p style="cursor:pointer" slot="author" @click="toUserInfo(item.comment.userid)">{{item.username}}</p>
                    <a-avatar
                      slot="avatar"
                      :src="item.avatar"
                      @click="toUserInfo(item.comment.userid)"
                    />
                    <p slot="content" style="text-align:left">{{ item.comment.content }}</p>
                    <a-tooltip
                      slot="datetime"
                      :title="item.comment.commenttime.format('YYYY-MM-DD HH:mm:ss')"
                    >
                      <span>{{ item.comment.commenttime.fromNow() }}</span>
                    </a-tooltip>
                  </a-comment>
                  <a-button
                    v-if="userinfo.userid === item.comment.userid || userinfo.userid === creatorid"
                    type="link"
                    size="small"
                    style="margin-top:-45px"
                    @click="handleDeleteComment(item)"
                  >删除</a-button>
                  <!-- 如果item的评论者或者该文档的所有者的userid 等于 当前userid，则该评论可删除 -->
                </a-list-item>
              </a-list>
              <a-row v-if="iscommentable">
                <a-col :span="4">
                  <div style="text-align:left">
                    <a-avatar slot="avatar" :src="userinfo.avatar" :alt="userinfo.username" />
                  </div>
                </a-col>
                <a-col :span="20" style="text-align:left;margin-bottom:-12px">
                  <div>
                    <a
                      style="margin:12px 0 0 0px;font-size:16px;padding-top:10px"
                      href="#/profile"
                    >{{userinfo.username}}</a>
                  </div>
                  <div style="font-size:12px;margin-left:0px">{{userinfo.description}}</div>
                </a-col>
              </a-row>

              <a-comment class="addComment" v-if="iscommentable">
                <div slot="content">
                  <a-form-item>
                    <a-textarea :rows="4" :value="comment" @change="handleChange" />
                  </a-form-item>
                  <a-form-item style="text-align:right">
                    <a-button html-type="submit" type="primary" @click="handleSubmitComment">评论</a-button>
                  </a-form-item>
                </div>
              </a-comment>
            </div>
          </div>
        </a-col>
      </a-row>
    </div>
  </div>
</template>

<style>
.text-editor {
  width: 1100px;
  background: #ffffff;
  margin: 24px auto;
}

.text-editor2 {
  border: 1px hsl(0, 0%, 82.7%) solid;
  border-radius: var(--ck-border-radius);
  box-shadow: 0 0 5px hsla(0, 0%, 0%, 0.1);
}

.subNavi {
  width: 100%;
  height: 56px;
  background: #fafafa;
  border-bottom: 1px rgb(236, 236, 236) solid;
}

.returnNarrow {
  font-size: 20px;
  margin-right: 10px;
  cursor: pointer;
}

.commentBox {
  width: 300px;
  margin: 24px auto;
  background: #ffffff;
  border: 1px hsl(0, 0%, 92.7%) solid;
  border-radius: var(--ck-border-radius);
  box-shadow: 0 0 5px hsla(0, 0%, 0%, 0.1);
}
</style>

<script>
export default {
  name: "Doc",
  components: {
    //InEditor,
    mavonEditor,
    qrcode: Qrcode,
  },
  data() {
    return {
      pagePath: window.location.href,
      tempauth: 0,
      tempteamauth: 1,
      submenuCurrent: "",
      groupAuthValue: 0,
      personAuthValue: 0,
      content: "", //输入的Markdown
      old_content: "",
      html: "", //渲染的html文件
      title: "Untitled",
      lastetidtimeString: "",
      creatorid: 0,
      editcount: 0,
      teamauth: 0,
      auth: 0,
      isTeammember: false,
      iseditable: false,
      iscommentable: false,
      isteamauth: false,
      teamid: 0,
      docid: this.$route.params.id,
      istrash: 0,
      isFav: 1,
      editrecord: [],
      // data - comment list, userinfo - 当前用户信息, value - 新评论
      data: [],
      userinfo: {},
      comment: "",
    };
  },
 
 }
</script>