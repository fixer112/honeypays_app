<template>
  <Page class="page">

    <ActionBar class="action-bar">
      <NavigationButton icon="res://baseline_menu_white_24" @tap="drawerTap"/>
      <Label text="Refund" style="text-align: center; font-weight: bold; color: white" />
      <ActionItem @tap="logout"
      ios.systemIcon="9" ios.position="left"
      android.systemIcon="ic_lock_power_off" android.position="actionBar" color="#fff"/>
    </ActionBar>


    <RadSideDrawer ref="drawer">
      <StackLayout ~drawerContent class="sidedrawer sidedrawer-content">
        <Drawer :mentor="mentor"></Drawer>
      </StackLayout>
      <StackLayout ~mainContent class="empower">
        <StackLayout>
              <FilterableListpicker ref="list" blur="dark" hintText="Type to filter..." :source="tran_list" @itemTapped="Tapped($event)"></FilterableListpicker>
              <FilterableListpicker ref="type" blur="dark" hintText="Type to filter..." :source="type_list" @itemTapped="typeTapped($event)"></FilterableListpicker>
              <FilterableListpicker ref="tenure" blur="dark" hintText="Type to filter..." :source="tenure_list" @itemTapped="tenureTapped($event)"></FilterableListpicker>
              <StackLayout class="input-field" marginBottom="25">
                <Label text="*Transaction" style="text-align: left;color:#000080" />
                <TextField class="input" :text="tran" editable="false" @tap="select()" fontSize="18"/>
                <StackLayout class="hr-light" />
              </StackLayout>

              <StackLayout class="input-field" marginBottom="25">
                <Label text="*Type" style="text-align: left;color:#000080" />
                <TextField class="input" :text="type_title" editable="false" @tap="typeSelect()" fontSize="18"/>
                <StackLayout class="hr-light" />
              </StackLayout>
              
              <StackLayout class="input-field" marginBottom="25">
                <Label text="*Tenure" style="text-align: left;color:#000080" />
                <TextField class="input" :text="tenure" editable="false" @tap="tenureSelect()" fontSize="18"/>
                <StackLayout class="hr-light" />
              </StackLayout>

            </StackLayout>

              <StackLayout>
                <Button text="SUBMIT" @tap="submit" class="button m-t-20" :isEnabled="!busy"/>
              </StackLayout>

            </StackLayout>

    </RadSideDrawer>
  </Page>
</template>

<script>
import axios from 'axios';
import Login from './Login';
import Land from '../LandPage';
export default {
  data () {
    return {
      mentor:"",
      subject:"",
      message:"",
      tran_list:[],
      tenure:'Select Tenure',
      type:'',
      type_title:'Select Title',
      type_list:[
          {'title':'One time payment','id':'1'},
          {'title':'Six time payment','id':'0'}
      ],
      tenure_list:[],
      id:'',
      tran:'Select Transaction',

    };
  },
  watch:{
    type(n){
      var list = [
        {"title": "5"},
        {"title": "9"},
        {"title": "18"},
        {"title": "36"},
      ];
      var six = [
        {"title": "18"},
        {"title": "36"},
      ];
      this.tenure_list = n == 1 ? list : six;
    }
  },
  methods:{
     select() {
        
        this.$refs.list.nativeView.show();
      },
      typeSelect() {
        
        this.$refs.type.nativeView.show();
      },
      tenureSelect() {
        
        this.$refs.tenure.nativeView.show();
      },
    Tapped(args) {
        this.id = args.selectedItem.id;
        this.tran = args.selectedItem.title;
        //console.log(args.selectedItem.id);
        
      },
      typeTapped(args) {
        this.type = args.selectedItem.id;
        this.type_title = args.selectedItem.title;
        //console.log(args.selectedItem.id);
        
      },
      tenureTapped(args) {
        this.tenure = args.selectedItem.title;
        //console.log(args.selectedItem.id);
        
      },
      submit(){
        if (this.id==""||this.message=="")
          {
            return alert('All * fields are reqired');
          }

          if (this.check_con()) {return}
          if (this.check_tym()) {return}
          this.show();
          this.busy= true;
          axios.post('https://empower.honeypays.com.ng/cus/roll',{
            tran:this.id,
            tenure:this.tenure,
            type:this.type,
            //subject:this.subject,
            //to:"refund@honeypays.com.ng"
          })
          .then(response => {
            this.hide();
            this.busy= false;
            console.log(response);
            alert(response.data.message);
            this.tran = "Select Transaction";
            this.message = "";
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
          var _this = this;
         this.mentor = this.$mentor;
         this.show();
            this.busy= true;
            axios.get('https://empower.honeypays.com.ng/cus/history')
            .then(response => {
              this.hide();
              this.busy= false;
              this.historys = response.data.historys;
              console.log(response.data.historys);
              response.data.historys.forEach(history => {
                this.tran_list.push(
                  {
                    'title': history["invest_amount"]+'-'+history["return_amount"]+' '+history["tran_id"],
                    'id' : history["id"],
                    }
                  );
              });
              //console.log(this.tran_list);
              
            })
            .catch((error)=>{
              this.hide();
              this.busy= false;
              console.log(error.response.data);
              this.danger('Error(s)', error);
            });
            
        }
      };
      </script>

      <style scoped>
      .input {
        font-size: 18;
        placeholder-color: #333;
      }
      .button{
        margin-right: 50;
        margin-left: 50;
        background-color: #000080;
        color: white;
        border-radius: 5;
        margin-top: 15;
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