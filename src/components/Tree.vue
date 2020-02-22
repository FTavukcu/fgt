<template>
    <li :class="filtered?'filtered':'unfiltered'">
        <div v-if="job.Objektname" :class="['treecontrol',expandable?collapsed&&!allexpanded?'expand':'collapse':'empty']"
            @click="toggle"/>
        <div v-for="(attr, index) in attrWithoutChildren(job)" :key="index" :class="'attr ' + index">
            <span v-if="index!='Status'">{{attr}}</span>
            <span v-else>
                <span :class="['traffic', attr.split('-')[0].trim()]"></span>
                {{attr.split('-')[1].trim()}}
            </span>
        </div>
        <ul>
            <tree v-for="(job, index) in job.Objekte" :key="index" :job="job" :filters="filters" :allexpanded="allexpanded"/>
        </ul>
    </li>
</template>

<style scoped>
.traffic {
    display: inline-block;
    width: 1em;
    height: 1em;
}

.traffic.ENDED_OK{
    background: #080;
}

.traffic.ENDED_INACTIVE{
    background: rgb(236, 176, 10);
}

.treecontrol { cursor: pointer }

.treecontrol.expand::after { content: '+' }
.treecontrol.collapse::after { content: '-' }
.treecontrol.empty::after { content: '.' }

.treecontrol.expand ~ ul * {
    display: none;
}

ul {
    padding: 1.2em 0 0 1em;
    list-style-type: none;
}


.treecontrol {
    height: 0;
    margin-left: -1em;
    user-select: none;
}

li.filtered {
    color: #AAA;
}

li.unfiltered {
    color: #000 !important;
}
</style>

<script>
export default {
    name: 'Tree',
    props: ['job', 'filters', 'allexpanded'],
    data: function() {
        return {
            expandable: this.job.Objekte?!!this.job.Objekte.length:false,
            collapsed: false
        }
    },
    methods: {
        toggle: function(){
            if (!this.allexpanded)
                this.collapsed = this.expandable?this.collapsed=!this.collapsed:false;
        },
        attrWithoutChildren: function(job) {
            var result = Object.assign({}, job);
            if (result.Objekte)
                delete result.Objekte;
            if (result.System)
                result = {};
            return result;
        }
    },
    computed: {
        filteredChildren: function() {
            var result = this.job.Objekte?this.job.Objekte.filter(obj => {
                var ret = obj.Objektname.indexOf(this.filters.Objektname)>=0;
                return ret;
            }):{};
            return result;
        },
        filtered: function() {
            var keys = Object.keys(this.filters);
            var ret = false;
            if (this.job.Objektname)
                keys.forEach(key => {
                    if (!ret)
                        ret = this.job[key].toLowerCase().indexOf(this.filters[key].toLowerCase()) == -1;
                });
            return ret;
        }
    }
}
</script>