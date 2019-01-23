<template>
    <div class="hello" style="height: calc(100vh);width: 100%; border: 0;padding: 0;">
        <el-row style="display: flex;flex-direction: row;">
            <div>
                <el-aside width="200px" style="background-color: rgb(238, 241, 246);height: 100%;">
                    <el-menu router style="height: calc(100vh)">
                        <el-menu-item index="overview">选项1</el-menu-item>
                    </el-menu>
                </el-aside>
            </div>
            <div style="width: 100%">
                <el-header style="height: 50px;width: 100%;background-color: #f8f8f8;"><b>I AM HEADER</b></el-header>
                <router-view></router-view>
            </div>
        </el-row>
    </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'home',
}
</script>
