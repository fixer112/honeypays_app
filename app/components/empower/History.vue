<template>
  <Page class="page">

            <ActionBar class="action-bar">
              <NavigationButton icon="res://baseline_menu_white_24" @tap="drawerTap"/>
        <Label text="History" style="text-align: center; font-weight: bold; color: white" />
        <ActionItem @tap="logout"
    ios.systemIcon="9" ios.position="left"
    android.systemIcon="ic_lock_power_off" android.position="actionBar" color="#fff"/>
            </ActionBar>
            

    <RadSideDrawer ref="drawer">
      <StackLayout ~drawerContent class="sidedrawer sidedrawer-content">
        <Drawer :mentor="mentor"></Drawer>
      </StackLayout>
      <StackLayout ~mainContent class="">
        <TabView androidTabsPosition="bottom" class="tab-view">

          <TabViewItem title="Active" iconSource="res://baseline_schedule_white_24">
            <StackLayout>
              <Label v-if="actives.length=='0'" text="No Active History" class="name" color="red" style="font-size: 20"></Label>
            
            <ListView class="list-group" for="active in actives">
            <v-template>
                  <StackLayout backgroundColor="#fafafa" class="list-group-item">
                    <ScrollView orientation="horizontal">
                    <StackLayout padding="5">
                      <AbsoluteLayout>
                      <Label text="Approved Date" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="active.approved_date" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Invest Date" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="active.invest_date" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Invest Amount" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="currency(active.invest_amount)" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Tenure" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="active.tenure" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Maturity Date" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="active.return_date" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Expected Return" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="currency(active.return_amount)" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Due for Payment" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="get_date(active.return_date) < get_date(now.date) ? 'Yes' : 'No'" :color="get_date(active.return_date) < get_date(now) ? 'green' : 'red'" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <StackLayout>
                      <Button text="View Proof" @tap="proof(active.proof)" class="button"/>
                    </StackLayout>
                    </StackLayout>
                  </ScrollView>
                  </StackLayout>

            </v-template>

          </ListView>
            </StackLayout>
          </TabViewItem>
          <TabViewItem title="Paid" iconSource="res://baseline_check_circle_outline_white_24">
            <StackLayout>
              <Label v-if="paids.length=='0'" text="No Paid History" class="name" color="red" style="font-size: 20"></Label>
            
            <ListView class="list-group" for="paid in paids">
            <v-template>
                  <StackLayout backgroundColor="#fafafa" class="list-group-item">
                    <ScrollView orientation="horizontal">
                    <StackLayout padding="5">
                      <AbsoluteLayout>
                      <Label text="Paid Date" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="paid.paid_date" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Invest Date" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="paid.invest_date" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Invest Amount" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="currency(paid.invest_amount)" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Tenure" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="paid.tenure" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Maturity Date" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="paid.return_date" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Amount Returned" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="currency(paid.return_amount)" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <StackLayout>
                      <Button text="View Proof" @tap="proof(paid.proof)" class="button"/>
                    </StackLayout>
                    </StackLayout>
                  </ScrollView>
                  </StackLayout>

            </v-template>

          </ListView>
            </StackLayout>
          </TabViewItem>
          <TabViewItem title="Pending" iconSource="res://baseline_info_white_24">
            <StackLayout>
              <Label v-if="pendings.length=='0'" text="No Pending History" class="name" color="red" style="font-size: 20"></Label>
            
                <ListView class="list-group" for="pending in pendings">
                <v-template>
                      <StackLayout backgroundColor="#fafafa" class="list-group-item">
                        <ScrollView orientation="horizontal">
                        <StackLayout padding="5">
                        <AbsoluteLayout>
                          <Label text="Invest Date" class="name" style="font-size: 14" top="10" left="10"></Label>
                          <Label :text="pending.invest_date" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                        </AbsoluteLayout>
                        <AbsoluteLayout>
                          <Label text="Invest Amount" class="name" style="font-size: 14" top="10" left="10"></Label>
                          <Label :text="currency(pending.invest_amount)" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                        </AbsoluteLayout>
                        <AbsoluteLayout>
                          <Label text="Tenure" class="name" style="font-size: 14" top="10" left="10"></Label>
                          <Label :text="pending.tenure" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                        </AbsoluteLayout>
                        <AbsoluteLayout>
                          <Label text="Expected Return" class="name" style="font-size: 14" top="10" left="10"></Label>
                          <Label :text="currency(pending.return_amount)" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                        </AbsoluteLayout>
                        <StackLayout>
                          <Button text="View Proof" @tap="proof(pending.proof)" class="button"/>
                        </StackLayout>
                        </StackLayout>
                      </ScrollView>
                      </StackLayout>

                </v-template>

              </ListView>
              </StackLayout>
          </TabViewItem>
          <TabViewItem title="Rejected" iconSource="res://baseline_highlight_off_white_24">
            <StackLayout>
              <Label v-if="rejecteds.length=='0'" text="No Rejected History" class="name" color="red" style="font-size: 20"></Label>
            
            <ListView class="list-group" for="reject in rejecteds">
            <v-template>
                  <StackLayout backgroundColor="#fafafa" class="list-group-item">
                    <ScrollView orientation="horizontal">
                    <StackLayout padding="5">
                      <AbsoluteLayout>
                      <Label text="Rejected Date" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="reject.approved_date" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Invest Date" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="reject.invest_date" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Invest Amount" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="currency(reject.invest_amount)" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Tenure" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="reject.tenure" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <AbsoluteLayout>
                      <Label text="Expected Return" class="name" style="font-size: 14" top="10" left="10"></Label>
                      <Label :text="currency(reject.return_amount)" style="text-align: right;font-size: 12" class="desc" top="10" left="200"></Label>
                    </AbsoluteLayout>
                    <StackLayout>
                      <Button text="View Proof" @tap="proof(reject.proof)" class="button"/>
                    </StackLayout>
                    </StackLayout>
                  </ScrollView>
                  </StackLayout>

            </v-template>

          </ListView>
            </StackLayout>
          </TabViewItem>
        </TabView>
       

    </StackLayout>


    </RadSideDrawer>
  </Page>
</template>

<script>
import axios from 'axios';
import Login from './Login';
import Land from '../LandPage';
import Proof from './Proof';
  export default {
    data () {
      return {
        user:"",
        actives:[],
        rejecteds:[],
        pendings:[],
        now:"",
        paids:[],

      };
    },
    methods:{
      proof(img){
        this.$showModal(Proof,{context:{propsData:{img:img}}});

      },
      get_user(){
            if (this.check_con()) {return}
            if (this.check_tym()) {return}
            this.show();
            this.busy= true;
            axios.get('https://empower.honeypays.com.ng/cus')
            .then(response => {
              this.hide();
              this.busy = false;
              this.user = response.data.user;
              this.actives = response.data.actives;
              this.rejecteds = response.data.rejecteds;
              this.pendings = response.data.pendings;
              this.now = response.data.now;
              this.paids = response.data.paids;
              console.log(response.data);
              /*this.$navigateTo(Home,{
              clearHistory:true,
              //backstackVisible:false,
            })*/
            })
            .catch((error)=>{
              this.hide();
              this.busy= false;
              console.log(error.response.data);
              this.danger('Error(s)', error);
            })
      },
      logout(){
            this.show();
            this.busy= true;
            axios.get('https://empower.honeypays.com.ng/logout')
            .then(response => {
              this.hide();
              this.busy= false;
              console.log(response.data);
              this.$navigateTo(Land,{
              clearHistory:true,
            })
            })
            .catch((error)=>{
              this.hide();
              this.busy= false;
              console.log(error.response.data);
              this.danger('Error(s)', error);
            })

      },
      drawerTap(){
        this.$refs.drawer.nativeView.toggleDrawerState();
      }
    },
    created(){
  this.get_user();
}
  };
</script>

<style scoped>
.button{
  margin-right: 50;
  margin-left: 50;
  background-color: #000080;
  color: white;
  border-radius: 5;
  margin-top: 15;
}
.name {
  color: black;
  font-family: 'Open Sans', 'OpenSans-Bold';
  font-weight: bold;
  padding: 0;
}

.desc {
  color: #000080;
  font-family: 'Open Sans', 'OpenSans-Regular';
  padding: 0;
}
.tab-view {
tab-background-color: #000080;
}
.user-avatar {
    border-color: #000080;
}
.stat{
  text-align: center;
}
.user-header {
  background-color: black;
  height: 290;
}
.ab-icon {
    color: white;
}

.page-content .page-placeholder {
    color: #D7D7D7;
    font-size: 20;
    vertical-align: center;
    horizontal-align: center;
}
.page-content .page-icon {
    font-size: 72;
    vertical-align: top;
    horizontal-align: center;
    color: #D7D7D7;
    margin-top: 20%;
}

.page-content {
    background-color: white;
}

.hr-light {
    background-color: #41464c;
}

.sidedrawer {
    background-color: #282c30;
}

.sidedrawer-header {
    background-color: #282c30;
    height: 190;
}

.sidedrawer-avatar {
    border-radius: 50%;
    width: 100;
    height: 100;
    border-width: 4;
    border-color: #48b1ec;
}

.sidedrawer-header-text {
    color: white;
    font-size: 24;
    font-weight: bold;
    margin-top: 10;
    text-align: center;
}

.sidedrawer-header-footnote {
    color: white;
    text-align: center;
    font-size: 16;
}

.sidedrawer-icon {
    font-size: 20;
    /*border-radius: 50%;
    border-width: 3;
    border-color: #393b3f;*/
    width: 20;
    height: 20;
    /*background-color: #393b3f;*/
    vertical-align: center;
    color: white;
    text-align: center;
}

.sidedrawer-list-item {
    color: #fff;
    padding-left: 30;
    font-size: 18;
    font-weight: bold;
    height: 60;
}

.sidedrawer-item {
    vertical-align: center;
    color: #fff;
}

.sidedrawer-list-item-active {
    /*background-color: #a6a450;*/
    background-color: black;
}

</style>