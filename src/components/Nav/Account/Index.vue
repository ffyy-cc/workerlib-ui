<script lang="ts">
    import "@/assets/css/common.css";
    import AccountStore from '../../../store/modules/AccountStore';
    import { Component, Vue, Prop, Model} from 'vue-property-decorator';
    import { getModule } from 'vuex-module-decorators';

    @Component({
        components:{
        },
        directives: { // 自定义指令
        },
        mounted() {
        },
        computed: {
            menuitemClasses: () => {
                return [
                    'menu-item',
                    this.isCollapsed ? 'collapsed-menu' : ''
                ]
            }
        }
    })
    export default class Account extends Vue {
        public addProject: boolean;
        private store: any;
        private options!: Array<any>;
        private uid:string;
        private updUser:boolean;
        public delUser: boolean;

        constructor() {
            super();
            this.addProject = false;
            this.store = getModule(AccountStore);
            this.updUser=false;
            this.delUser = false;
        }
        rowClassName (row, index) : string {
            if(index == 0) {
                return 'table-header'
            }
            return '';
        }
        mounted() {
            this.store.search();

        }
        public search() {
            this.store.search();
        }
        ok() : any{
            this.store.insertUser();
            this.addProject = false;
        }
        yes() : any{
            this.store.updateUser();
        }
        popupDelUser(userId){
            this.store.setUid(userId);
            // this.store.deleteUser(userId);
            this.delUser =! this.delUser;
        }
        deletes() : any{
            this.store.deleteUser();
        }
        popupUpdUser(userId){
            this.uid = userId;
            this.store.setUid(this.uid);
            this.updUser = !this.updUser;
            // this.store.updateUser(userId);
        }
        cancel():any {
            this.addProject = false;
        }

        onPageSizeChange(pageSize){

            this.store.pageSize(pageSize);
            this.store.pageIndex(1);
            this.onPageIndexChange(1);
        }

        onPageIndexChange(pageIndex){

            console.log(pageIndex)

            this.store.pageIndex(pageIndex);
            this.store.search();
        }

        getColumns() : any{
            return this.store.columns;
        }
        getData() : any{
            return this.store.project;
        }


        get username():string{
            return this.store.userInfo.username;
        }
        set username(data:string){
            this.store.setUsername(data);
        }
        get password():string{
            return this.store.userInfo.password;
        }
        set password(data:string){
            this.store.setPassword(data);
        }

    }
</script>
<style scoped src="@/styles/account.css" />
<template lang="pug" src="@/views/account.pug" />
