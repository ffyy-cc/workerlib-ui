<script lang="ts">
    import "@/assets/css/common.css";
    import CoursewareStore from '../../../store/modules/CoursewareStore';
    import { Component, Vue, Prop, Model} from 'vue-property-decorator';
    import { getModule } from 'vuex-module-decorators';
    import { Message } from 'iview';

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
    export default class Courseware extends Vue {
        @Model('isCollapsed', { type: Boolean }) private isCollapsed !: boolean;
        mounted() {
            this.store.search();
            this.store.getProjectType();
        }

        single = true;
        loading = true;
        public indeterminate: boolean;
        public checkAll: boolean;
        public addCourseware: boolean;
        public addUpCourseware: boolean;
        public deleteCourseware: boolean;
        public updateCourseware: boolean;
        public addCultivate: boolean;
        public pageName: string;
        public pageUpName: string;
        private store: any;
        public id:number;
        public checkAllGroup :Array<any>;
        public onTitle:string;
        constructor() {
            super();
            this.onTitle = null;
            this.indeterminate= true;
            this.checkAll = false;
            this.addCultivate = false;
            this.addCourseware = false;
            this.addUpCourseware = false;
            this.updateCourseware = false;
            this.pageName = "name1";
            this.pageUpName = "name1";
            this.deleteCourseware = false;
            this.id = null;
            this.checkAllGroup = [];
            this.store = getModule(CoursewareStore)

        }
        rowClass(row, index) {
            return "rowClasses"
        }

        search(){
            this.store.searchPeople();
        }
        getColumns() : any{
            return this.store.columns;
        }
        getData() : any{
            for(let i=0;i<this.store.peoples.length;i++) {
                if(this.store.checkeds.filter(a => a.id == this.store.peoples[i].id).length > 0){
                    this.$set(this.store.peoples[i], '_disabled', true)
                }
            }
            return this.store.peoples;
        }

        getCourseWareList():any{
            return this.store.courseWareInfo;
        }
        getCourseware() : any{
            return this.store.courseware;
        }
        messageWarningFn (text) {
            let alert: any = Message;
            alert.warning(text);
            setTimeout(() => {
                this.loading = false;
                this.$nextTick(() => {
                    this.loading = true;
                })
            }, 500)
        }
        ok() : any{
            this.store.setTeachingMethod("录播");
            if(this.store.courseWare.title == "" || this.store.courseWare.title == null ){
                this.messageWarningFn('请输入课件名称！');
                return;
            }
            if(this.store.courseWare.course == "" || this.store.courseWare.course == null ){
                this.messageWarningFn('请选择课程！');
                return;
            }
            if(this.store.courseWare.total_hours == "" || this.store.courseWare.total_hours == null ){
                this.messageWarningFn('请输入总课时！');
                return;
            }
            if(this.store.courseWare.type_work == "" || this.store.courseWare.type_work == null ){
                this.messageWarningFn('请绑定课件工种分类！');
                return;
            }
            // if(this.store.courseWare.video == "" || this.store.courseWare.video == null ){
            //     this.messageWarningFn('请上传资料！');
            //     this.pageName = "name2"
            //     return;
            // }
            this.store.setStatus(1);
            this.store.insertCourseware();
            this.store.clearCourseWare();
            this.addCourseware = false;
        }
        cancel():any {
            debugger
            this.store.clearCourseWare();
            this.addCourseware = false;
        }
        okUp() : any{
            this.store.setTeachingMethod("现场培训");
            if(this.store.courseWare.title == "" || this.store.courseWare.title == null ){
                this.messageWarningFn('请输入课件名称！');
                return;
            }
            if(this.store.courseWare.course == "" || this.store.courseWare.course == null ){
                this.messageWarningFn('请选择课程！');
                return;
            }
            if(this.store.courseWare.total_hours == "" || this.store.courseWare.total_hours == null ){
                this.messageWarningFn('请输入总课时！');
                return;
            }
            if(this.store.courseWare.type_work == "" || this.store.courseWare.type_work == null ){
                this.messageWarningFn('请绑定课件工种分类！');
                return;
            }
            // if(this.store.courseWare.video == "" || this.store.courseWare.video == null ){
            //     this.messageWarningFn('请上传资料！');
            //     this.pageName = "name2"
            //     return;
            // }
            this.store.setStatus(2);
            this.store.insertCourseware();
            this.store.clearCourseWare();
            this.addUpCourseware = false;
        }
        cancelUp():any {
            this.store.clearCourseWare();
            this.addUpCourseware = false;
        }

        okEdit() : any{
            debugger

            if(this.store.courseWareEdit.title == "" || this.store.courseWareEdit.title == null ){
                this.messageWarningFn('请输入课件名称！');
                return;
            }
            if(this.store.courseWareEdit.course == "" || this.store.courseWareEdit.course == null ){
                this.messageWarningFn('请选择课程！');
                return;
            }
            if(this.store.courseWareEdit.total_hours == "" || this.store.courseWareEdit.total_hours == null ){
                this.messageWarningFn('请输入总课时！');
                return;
            }
            if(this.store.courseWareEdit.type_work == "" || this.store.courseWareEdit.type_work == null ){
                this.messageWarningFn('请绑定课件工种分类！');
                return;
            }
            // if(this.store.courseWare.video == "" || this.store.courseWare.video == null ){
            //     this.messageWarningFn('请上传资料！');
            //     this.pageName = "name2"
            //     return;
            // }
            this.store.updateCourseware();
            this.updateCourseware = false;
        }
        cancelEdit():any {
            this.updateCourseware = false;
        }

        okDel() : any{
            this.store.setId(this.id);
            this.store.deleteCourseware();
            this.deleteCourseware = false;
        }
        cancelDel():any {
            this.deleteCourseware = false;
        }

        okAdd() : any{
            this.store.setPeoples(this.store.checkeds.length);
            this.store.setState("待学习");
            for(let i = 0; i< this.store.checkeds.length;i++){
                var itemTrue = {};
                itemTrue['archives_id'] = Number(this.store.checkeds[i].id);
                itemTrue['cultivate_id'] = this.store.cultivate.course_id;
                this.store.setCultivateArchivesList(itemTrue);
            }
            this.store.insertCultivate();
            this.addCultivate = false;
        }
        cancelAdd():any {
            this.addCultivate = false;
        }

        offChecked(id): boolean {
            if(!id) return;
            if(this.store.checkeds.findIndex(x => x.id == id) > -1){
                return false;
            }else {
                return true;
            }
        }
        // 单选
        handleSelectRow(selection, row) {
            let index = this.store.checkeds.findIndex(x => x.id == row.id);
            if(index > -1) {
                let indexPeople = this.store.peoples.findIndex(x => x.id == row.id);
                if(indexPeople > -1)  {
                    this.$set(this.store.peoples[indexPeople], '_disabled', false);
                }
                this.store.checkeds.splice(index, 1);
                return;
            }
            var itemTrue = {};
            itemTrue['id'] = row.id;
            itemTrue['name'] = row.name;
            itemTrue['photo'] = row.photo;
            this.checkAllGroup.push(itemTrue);
        }

        //多选
        handleSelectAll(selection) {
            for(let i= 0;i<selection.length;i++){
                var itemTrue = {};
                let row = selection[i];
                itemTrue['id'] = row.id;
                itemTrue['name'] = row.name;
                itemTrue['photo'] = row.photo;
                this.checkAllGroup.push(itemTrue);
            }
        }
        addSelected(){
            for (let i=0;i<this.checkAllGroup.length;i++){
                var itemTrue = {};
                let row = this.checkAllGroup[i];
                itemTrue['id'] = row.id;
                itemTrue['name'] = row.name;
                itemTrue['photo'] = row.photo;
                this.store.setChecked(itemTrue);
                let index = this.store.peoples.findIndex(x => x.id == row.id);
                this.$set(this.store.peoples[index], '_disabled', true)
            }
            this.checkAllGroup = [];
        }
        show(id: number,name:string,photo:string): void {
            debugger
            let _that = this;
            var itemTrue = {};
            let index = this.store.checkeds.findIndex(x => x.id == id);
            if(index > -1) {
                let indexPeople = this.store.peoples.findIndex(x => x.id == id);
                if(indexPeople > -1)  {
                    console.log(this.store.peoples[indexPeople]._disabled);
                    this.$set(this.store.peoples[indexPeople], '_disabled', false);
                    console.log(this.store.peoples[indexPeople]._disabled);
                }
                this.store.checkeds.splice(index, 1);
                return;
            }
            itemTrue['id'] = id;
            itemTrue['name'] = name;
            itemTrue['photo'] = photo;
            this.store.setChecked(itemTrue);
            for (let i=0;i<this.store.checkeds.length;i++){
                let row = this.store.checkeds[i];
                let index = this.store.peoples.findIndex(x => x.id == row.id);
                this.$set(this.store.peoples[index], '_disabled', true)
            }
        }
        toggle(name){
            if(name=="线上培训"){
                this.store.setSelectStatus(1);
                this.store.search();
            }else {
                this.store.setSelectStatus(2);
                this.store.search();
            }
        }
        private options!: any;
        getMenus() : any {
            if(this.options) return this.options;
            this.options = [
                { value: '在职', key: 1 },
                { value: '离职', key: 2 }
            ];
            return this.options;
        }

        getType(){
            return this.store.projectType.filter(x => x.category === "工种");
        }
        getCType(){
            return this.store.projectType.filter(x => x.category === "课程");
        }
        change(name){
            this.id= name.split('_')[1];
            if(name.split('_')[0] == 'edit') {
                this.store.setEditId(this.id);
                this.store.searchInfo();
                this.updateCourseware = true;
            }else {
                this.deleteCourseware = true;
            }
        }
        viewData(id,title) {
            this.addCultivate=!this.addCultivate;
            this.store.setCourseId(id);
            this.store.setCourseName(title)
            this.onTitle = title;
            this.store.searchPeople();
        }
        handleSuccessVideo (res, file) {
            this.store.setVideo(res.file);
        }
        handleFormatError (file) {
            let alert: any = Message;
            alert.warning(file.name + ' 文件格式错误！请上传AVI、mov、rmvb、rm、FLV、mp4、3GP格式文件！');
        }
        handleFormatPictrueError (file) {
            let alert: any = Message;
            alert.warning(file.name + ' 文件格式错误！请上传jpg、jpeg、png格式文件！');
        }
        handleSuccessPicture (res, file) {
            console.log(res.file)
            this.store.setCoverPicture(res.file);
        }
        onPageSizeChange(pageSize){
            this.store.setPageSize(pageSize);
            this.store.setPageIndex(1);
            this.onPageIndexChange(1);
        }
        onPageIndexChange(pageIndex){
            this.store.setPageIndex(pageIndex);
            this.store.search();
        }
        onPageSizeInChange(pageSize){
            this.store.setInPageSize(pageSize);
            this.store.setInPageIndex(1);
            this.onPageIndexInChange(1);
        }
        onPageIndexInChange(pageIndex){
            this.store.setInPageIndex(pageIndex);
            this.store.searchPeople();
        }

        set pageTotal(data:number){
            this.store.setPageToatl(data);
        }
        get pageTotal():number{
            return this.store.pageTotal;
        }

        set pageInTotal(data:number){
            this.store.setPageToatl(data);
        }
        get pageInTotal():number{
            return this.store.pageInTotal;
        }


        set selectTypeWork(data:string){
            this.store.setSelectTypeWork(data);
        }
        get selectTypeWork():string{
            return this.store.selectTypeWork;
        }

        set selectTitle(data:string){
            this.store.setSelectTitle(data);
        }
        get selectTitle():string{
            return this.store.selectTitle;
        }

        set status(data:number){
            this.store.setStatus(data);
        }
        get status():number{
            return this.store.courseWare.status;
        }

        set title(data:string){
            this.store.setTitle(data);
        }
        get title():string{
            return this.store.courseWare.title;
        }

        set course(data:string){
            this.store.setCourse(data);
        }
        get course():string{
            return this.store.courseWare.course;
        }

        set totalHours(data:string){
            this.store.setTotalHours(data);
        }
        get totalHours():string{
            return this.store.courseWare.total_hours;
        }

        set teachingMethod(data:string){
            this.store.setTeachingMethod(data);
        }
        get teachingMethod():string{
            return this.store.courseWare.teaching_method;
        }

        set whether(data:string){
            this.store.setWhether(data);
        }
        get whether():string{
            return this.store.courseWare.whether;
        }

        set typeWork(data:string){
            this.store.setTypeWork(data);
        }
        get typeWork():string{
            if(!this.store.courseWareEdit.type_work) {
                return '';
            }
            return this.store.courseWareEdit.type_work.toString().split(",");
        }

        set describe(data:string){
            this.store.setDescribe(data);
        }
        get describe():string{
            return this.store.courseWare.describe;
        }

        set particulars(data:string){
            this.store.setParticulars(data);
        }
        get particulars():string{
            return this.store.courseWare.particulars;
        }

        set editId(data:number){
            this.store.setEditId(data);
        }
        get editId():number{
            return this.store.courseWareEdit.id;
        }

        set editTitle(data:string){
            this.store.setEditTitle(data);
        }
        get editTitle():string{
            return this.store.courseWareEdit.title;
        }

        set editCourse(data:string){
            this.store.setEditCourse(data);
        }
        get editCourse():string{
            return this.store.courseWareEdit.course;
        }

        set editTotalHours(data:string){
            this.store.setEditTotalHours(data);
        }
        get editTotalHours():string{
            return this.store.courseWareEdit.total_hours;
        }

        set editTeachingMethod(data:string){
            this.store.setEditTeachingMethod("录播");
        }
        get editTeachingMethod():string{
            return this.store.courseWareEdit.teaching_method;
        }

        set editWhether(data:string){
            this.store.setEditWhether("是");
        }
        get editWhether():string{
            return this.store.courseWareEdit.whether;
        }

        set editTypeWork(data:string){
            this.store.setEditTypeWork(data);
        }
        get editTypeWork():string{
            if(!this.store.courseWareEdit.type_work) {
                return '';
            }
            return this.store.courseWareEdit.type_work.toString().split(",");
        }

        set editDescribe(data:string){
            this.store.setEditDescribe(data);
        }
        get editDescribe():string{
            return this.store.courseWareEdit.describe;
        }

        set editVideo(data:string){
            this.store.setEditVideo(data);
        }
        get editVideo():string{
            return this.store.courseWareEdit.video;
        }

        set editParticulars(data:string){
            this.store.setEditParticulars(data);
        }
        get editParticulars():string{
            return this.store.courseWareEdit.particulars;
        }

        set selectProjectName(data:string){
            this.store.setSelectProjectName(data);
        }
        get selectProjectName():string{
            return this.store.courseWareEdit.selectProjectName;
        }

        set selectUserName(data:string){
            this.store.setSelectUserName(data);
        }
        get selectUserName():string{
            return this.store.courseWareEdit.selectUserName;
        }

        set selectLeave(data:string){
            this.store.setSelectLeave(data);
        }
        get selectLeave():string{
            return this.store.courseWareEdit.selectLeave;
        }
        //-------------------------------------
        set courseId(data:string){
            this.store.setCourseId(data);
        }
        get courseId():string{
            return this.store.cultivate.course_id;
        }

        set startTime(data:string){
            this.store.setStartTime(data);
        }
        get startTime():string{
            return this.store.cultivate.start_time;
        }

        set peoples(data:string){
            this.store.setPeoples(data);
        }
        get peoples():string{
            return this.store.cultivate.peoples;
        }

        set state(data:string){
            this.store.setState(data);
        }
        get state():string{
            return this.store.cultivate.state;
        }

        set mark(data:string){
            this.store.setMark(data);
        }
        get mark():string{
            return this.store.cultivate.mark;
        }
        set coursewareBrief(data:string){
            this.store.setCoursewareBrief(data);
        }
        get coursewareBrief():string{
            return this.store.cultivate.courseware_brief;
        }
    }
</script>
<style scoped src="@/styles/courseware.css" />
<template lang="pug" src="@/views/courseware.pug" />
