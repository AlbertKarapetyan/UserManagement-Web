<template>
    <div :className="user.isActive == true ? '' : 'inactive'" @click="selectUser(user)">
        <div className="row">
            <h3 :className="user.username == currentUsername && user.isActive == false ? 'warn' : user.username == currentUsername ? 'current' : ''">
                {{ user.username }}
            </h3>    
        </div>
        <div className="row status">{{ user.isActive == true ? 'Yes' : 'No' }}</div>
    </div>

</template>

<script>
export default {
    props: {
        user: {
            type: Object,
            Required: true
        },
        currentUsername:{
            type: String,
            Required: true
        },
        id: {
            type: Number,
            Required: true
        },
        selectUser: {
            type: Function,
            Required: true
        }
    }
}
</script>

<style scoped>
h3 {
    font-size: 13px;
    font-weight: bold;
    font-family: 'Nabla', cursive;
    color: rgb(185, 185, 15);
}

h3.current{
    color: #9993df;
    font-style: oblique;
}

h3.warn {
    color: #f1f1f4;
    background: #b91212;
    padding: 0px 5px;
}

.row {
    width: 200px;
    padding: 5px;
    margin: 3px;
    border-radius: 3px;
    border-bottom: 1px solid #e4ba12;
    float: left;
    justify-content: space-around;
}

div.inactive {
    color: red !important;
}

.status {
    width: 100px;
    text-align: center;
}
</style>