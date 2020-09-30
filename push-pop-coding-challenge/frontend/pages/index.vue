<template>
  <div>

    <div>
      <button 
       type="button" 
       class="intersection" 
       style="color:#04771C;background-color: #F8EAE7;border: 2px solid #414141;border-radius: 8px;float: middle;" 
      v-on:click="Intersection()">Show Intersection</button>
      <button 
       type="button" 
       class="intersection" 
       style="color:#04771C;background-color: #F8EAE7;border: 2px solid #414141;border-radius: 8px;float: middle;" 
      v-on:click="HideIntersection()">Hide Intersection</button>
    </div>

    <div>
      <p class="inter" id="inter" style="color:#04771C;font-size: 15px">  {{ inter_list }}  </p>
    </div>

    <div>
       <button 
       type="button" 
       class="UndButton" 
       style="color:#FF0000;background-color: #F8EAE7;border: 2px solid #414141;border-radius: 12px;float: right;" 
      v-on:click="undobutton()">Undo</button>
    </div>
    <div>
      <p style="color:#FF0000;font-size: 20px">Unresolved : {{ unresolved.length }}  </p>
    </div>
    <div>
    <vue-good-table class="vgtable"
      :columns="columns"
      :rows="unresolved"
      max-height="275px"
      :sort-options="{enabled: false}"
      :fixed-header="true">
      <template slot="table-row" slot-scope="props">
        <span v-if="props.column.field == 'action'">
          <button type="button" class="myButton" style="color:#4D9905;background-color: #F2F6E1;border: 2px solid #414141;border-radius: 12px;" 
          v-on:click="buttonclick(props.index,2)">Resolve</button>
        </span>
        <span v-else>
          {{props.formattedRow[props.column.field]}}
        </span>
      </template>
    </vue-good-table>
    </div>

    <div>
       <button 
       type="button" 
       class="UndButton" 
       style="color:#FF0000;background-color: #F8EAE7;border: 2px solid #414141;border-radius: 12px;float: right;" 
      v-on:click="undobutton()">Undo</button>
    </div>

    <div>
      <p style="color:#FF0000;font-size: 20px">Resolved : {{ resolved.length }} </p>
    </div>

    <div>
    <vue-good-table class="vgtable"
      :columns="columns"
      :rows="resolved"
       max-height="275px"
      :fixed-header="true"
      :sort-options="{enabled: false}">
      <template slot="table-row" slot-scope="props">
        <span v-if="props.column.field == 'action'">
          <button type="button" class="myButton" style="color:#FF0000;background-color: #F8EAE7;border: 2px solid #414141;border-radius: 12px;" 
          v-on:click="buttonclick(props.index,1)">Unresolve</button>
        </span>
        <span v-else>
          {{props.formattedRow[props.column.field]}}
        </span>
      </template>
    </vue-good-table>
    </div>

    <div>
       <button 
       type="button" 
       class="UndButton" 
       style="color:#FF0000;background-color: #F8EAE7;border: 2px solid #414141;border-radius: 12px;float: right;" 
      v-on:click="undobutton()">Undo</button>
    </div>

    <div>
      <p style="color:#FF0000;font-size: 20px">Backlog : {{ backlog.length }}  </p>
    </div>

    <div>
    <vue-good-table class="vgtable"
      :columns="columns"
      :rows="backlog"
      :sort-options="{enabled: false}"
      max-height="275px"
      :fixed-header="true">
      <template slot="table-row" slot-scope="props">
        <span v-if="props.column.field == 'action'">
          <button type="button" class="myButton" style="color:#FB6103;background-color: #FAE3D6;border: 2px solid #414141;border-radius: 12px;" 
          v-on:click="buttonclick(props.index,3)">Unresolve</button>
        </span>
        <span v-else>
          {{props.formattedRow[props.column.field]}}
        </span>
      </template>
    </vue-good-table>
    </div>

    <div>
      <client-only>
      <notifications group="notifiy" position="bottom left"/>
      </client-only>
    </div>

    </div>
</template>

<script>
import axios from 'axios';
import Vue from 'vue';
import { VueGoodTable } from 'vue-good-table';
import 'vue-good-table/dist/vue-good-table.css';
export default {
  components: {
      VueGoodTable,
  },
  async asyncData({ $axios }) {
    try {
      const { resolved, unresolved, backlog } = await $axios.$get(
        "http://localhost:8000/get_lists"
      );
      return {
        resolved,
        unresolved,
        backlog
      };
    } catch (error) {
      console.log(
        `Couldn't get error lists:\n${error}\nDid you start the API?`
      );
    }
  },
  data() {
    return {
      resolved: [],
      unresolved: [],
      backlog: [],
      columns: [ {
          label: 'Index',
          field: 'index',
          type: 'number',
        },
        {
          label: 'Errorcode',
          field: 'code',
          type: 'number',
        },
        {
          label: 'Text',
          field: 'text',
          type: 'text',
        },
        {
          label: 'Action',
          field: 'action',
        }],
      undo_list: [],
      inter_list: [],
      show:0
    };
  },
  link: [
          {
            rel: 'stylesheet',
            href: './node_modules/vue-good-table/dist/vue-good-table.css'
          }
        ],
  methods: {
    buttonclick: function(idx,no){ 
        if (no == 1){ 
          var row = this.resolved[idx];  
          this.resolved.splice(idx,1);   
          this.unresolved.push(row);  
          this.$notify({'group':'notifiy',type: 'success','title':'Unresolved!!','text':row});   
          }
        else if (no == 2){ 
          var row = this.unresolved[idx];  
          this.unresolved.splice(idx,1);   
          this.resolved.push(row);    
          this.$notify({'group':'notifiy',type: 'success','title':'Resolved!!','text':row});      
          }
        else if (no == 3){ 
          var row = this.backlog[idx];      
          this.backlog.splice(idx,1);      
          this.unresolved.push(row);    
          this.$notify({'group':'notifiy',type: 'success','title':'Unresolved!!','text':row});     
          }
        this.undo_list.push([no,row.index]);

        axios({
              method: 'post',
              url: 'http://localhost:8000/get_error_request',
              params:{name:row,activity:no},
              }).catch(function (error) {
                       console.log(error);
                   });
        if (this.show == 1)
            this.Intersection();
      
    },
    undobutton: function(){
      if (this.undo_list == 0){
        this.undo_list = [];
        this.$notify({'group':'notifiy',type: 'error','title':'Undo Request','text':'No Action to Undo!!'});

        axios({
              method: 'post',
              url: 'http://localhost:8000/get_undo_request',
              params:{name:"",activity:0},
              }).catch(function (error) {
                       console.log(error);
                   });

      }
      else {
              var undo_val = this.undo_list;
              var len = undo_val.length
              var last_undo = undo_val[len-1];
              this.undo_list.splice(len-1,1)
              var undo_action = last_undo[0];
              var undo_index = last_undo[1];
            if (undo_action == 1){
              for (var i = 0; i < this.unresolved.length; i++) 
              {
                var row = this.unresolved[i];
                if (row.index == undo_index){
                  this.unresolved.splice(i,1);
                  this.resolved.push(row);
                  break;
                }
              }
            }
            else if (undo_action == 2){
              for (var i = 0; i < this.resolved.length; i++) 
              {
                var row = this.resolved[i];
                if (row.index == undo_index){
                  this.resolved.splice(i,1);
                  this.unresolved.push(row);
                  break;
                }
              }
            }
            else if (undo_action == 3){
              for (var i = 0; i < this.unresolved.length; i++) 
              {
                var row = this.unresolved[i];
                if (row.index == undo_index){
                  this.unresolved.splice(i,1);
                  this.backlog.push(row);
                  break;
                }
              }
            }
        this.$notify({'group':'notifiy',type: 'warn','title':'Undo Request - Successfull!!','text':row});

        axios({
              method: 'post',
              url: 'http://localhost:8000/get_undo_request',
              params:{name:row,activity:undo_action},
              }).catch(function (error) {
                       console.log(error);
                   });

        if (this.show == 1)
            this.Intersection();
        }
    },

    HideIntersection: function(){ 
      this.show = 0;
      this.inter_list = []
    },  

    Intersection_display: function(response){ 
      this.inter_list = []
      this.inter_list.push(response);
    }, 

    Intersection: function(){ 
        this.show = 1;
        axios({method: 'get',
                  url: 'http://localhost:8000/get_list_intersection_counts',
                  params:{err:{resolved:this.resolved,unresolved:this.unresolved,backlog:this.backlog}},
                  }).then(response => {
                    this.Intersection_display(response.data); 
                  }).catch(function (error) {
                    console.log(error);
                  });
    }
    },
};
</script>
