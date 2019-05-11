<template>
    <ScrollView>
        <StackLayout>
          
          <StackLayout class="hr-light"></StackLayout>
          <!-- sidedrawer-list-item-active -->
          <GridLayout columns="auto, *" class="sidedrawer-list-item" title="Home">
            <Image src="~/assets/icons/home.png" class="sidedrawer-icon"/>
            <!-- <Label row="0" col="0" text="&#xf015;" class="fa sidedrawer-icon" /> -->
            <Label row="0" col="1" text="Home" class="m-l-20 sidedrawer-item" />
          </GridLayout>

          <StackLayout class="hr-light"></StackLayout>

          <GridLayout columns="auto, *" class="sidedrawer-list-item" title="History" @tap="onNavigationItemTap('history')">
            <Image src="~/assets/icons/history.png" class="sidedrawer-icon"/>
            <Label row="0" col="1" text="History" class="m-l-20 sidedrawer-item"/>
          </GridLayout>

          <StackLayout class="hr-light"></StackLayout>

          <GridLayout columns="auto, *" class="sidedrawer-list-item" title="Edit" @tap="onNavigationItemTap('edit')">
            <Image src="~/assets/icons/edit.png" class="sidedrawer-icon"/>
            <Label row="0" col="1" text="Edit" class="m-l-20 sidedrawer-item" />
          </GridLayout>

          <StackLayout class="hr-light"></StackLayout>

          <GridLayout columns="auto, *" class="sidedrawer-list-item" title="Invest" @tap="onNavigationItemTap('invest')">
            <Image src="~/assets/icons/money.png" class="sidedrawer-icon"/>
            <Label row="0" col="1" text="Invest" class="m-l-20 sidedrawer-item" />
          </GridLayout>

          <StackLayout class="hr-light"></StackLayout>

          <GridLayout columns="auto, *" class="sidedrawer-list-item" title="Calculator" @tap="onNavigationItemTap('cal')">
              <Image src="~/assets/icons/cal.png" class="sidedrawer-icon"/>
              <Label row="0" col="1" text="Calculator" class="m-l-20 sidedrawer-item" />
            </GridLayout>

            <StackLayout class="hr-light"></StackLayout>

          <GridLayout columns="auto, *" class="sidedrawer-list-item" title="Contact" @tap="onNavigationItemTap('contact')">
            <Image src="~/assets/icons/contact.png" class="sidedrawer-icon"/>
            <Label row="0" col="1" text="Contact" class="m-l-20 sidedrawer-item" />
          </GridLayout>

          <StackLayout class="hr-light"></StackLayout>

          <GridLayout columns="auto, *" class="sidedrawer-list-item" title="Refund" @tap="onNavigationItemTap('refund')">
              <Image src="~/assets/icons/refund.png" class="sidedrawer-icon"/>
              <Label row="0" col="1" text="Refund" class="m-l-20 sidedrawer-item" />
            </GridLayout>

        <StackLayout class="hr-light"></StackLayout>

          <GridLayout columns="auto, *" class="sidedrawer-list-item" title="Roll Over" @tap="onNavigationItemTap('roll')">
              <Image src="~/assets/icons/refund.png" class="sidedrawer-icon"/>
              <Label row="0" col="1" text="Roll Over" class="m-l-20 sidedrawer-item" />
            </GridLayout>

            <StackLayout class="hr-light"></StackLayout>

          <GridLayout v-if="mentor!=''" columns="auto, *" class="sidedrawer-list-item" title="Referal" @tap="onNavigationItemTap('referal')">
            <Image src="~/assets/icons/referal.png" class="sidedrawer-icon"/>
            <Label row="0" col="1" text="Referal" class="m-l-20 sidedrawer-item" />
          </GridLayout>

          <StackLayout class="hr-light"></StackLayout>

        </StackLayout>
      </ScrollView>
      </template>
      <script>
      export default {
          props:['mentor'],
  data () {
    return {
       // mentor:"",
    };
  },
  created(){
      
  }
      }
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
      