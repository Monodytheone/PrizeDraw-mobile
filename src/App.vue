<template>
    <div>
        <NotStart v-if="sysStatus === 1" />
        <PrizeListPage v-if="sysStatus === 2 || sysStatus === 5" :data="sysStatusRes.started_2_5" :hideEmployeeInfo="hideEmployeeInfo" />
        <RollingPage v-if="sysStatus === 3" :data="sysStatusRes.rolling_3" :hideEmployeeInfo="hideEmployeeInfo" />
        <RollStopPage v-if="sysStatus === 4" :data="sysStatusRes.stopRoll_4" :hideEmployeeInfo="hideEmployeeInfo" />
    </div>
</template>

<script setup>
import NotStart from './views/NotStart.vue';
import PrizeListPage from './views/PrizeListPage.vue';
import RollingPage from './views/RollingPage.vue';
import RollStopPage from './views/RollStopPage.vue';
import { getSysStatusFromCache } from './api/home';
import { onMounted, reactive } from 'vue';
import * as signalR from '@microsoft/signalr';

const sysStatus = ref(0); // 系统状态
const sysStatusRes = ref();
const hideEmployeeInfo = ref(false);

let signalrConnection;

onMounted(async () => {
    sysStatusRes.value = await getSysStatusFromCache();
    sysStatus.value = sysStatusRes.value.sysStatus;
    hideEmployeeInfo.value = sysStatusRes.value.hideEmployeeInfo;

    const options = { skipNegotiation: true, transport: signalR.HttpTransportType.WebSockets };
    signalrConnection = new signalR.HubConnectionBuilder()
        .withUrl(import.meta.env.VITE_PROXY_DOMAIN_SOCKET, options)
        .withAutomaticReconnect()  // 自动重连
        .build();
    await signalrConnection.start();


    signalrConnection.on('Start', data => {
        sysStatus.value = 2;
        sysStatusRes.value.started_2_5 = data;
    });

    signalrConnection.on('StartRoll', data => {
        sysStatus.value = 3;
        sysStatusRes.value.rolling_3 = data;
    });

    signalrConnection.on('StopRoll', data => {
        sysStatus.value = 4;
        sysStatusRes.value.stopRoll_4 = data;
    });

    signalrConnection.on('BackToPrizeListPage', data => {
        sysStatus.value = 2;
        sysStatusRes.value.started_2_5 = data;
    });

    signalrConnection.on('Finish', data => {
        sysStatus.value = 5;
        sysStatusRes.value.started_2_5 = data;
    });

    signalrConnection.on('SystemReset', data => {
        sysStatus.value = 1;
    });

    signalrConnection.on('ChangeHideEmployeeInfoStatus', data => {
        hideEmployeeInfo.value = data;
    })
})

</script>


<!-- <style scoped>
header {
    max-height: 100vh;
    line-height: 1.5;
}

.logo {
    display: block;
    margin: 0 auto 2rem;
}

nav {
    width: 100%;
    margin-top: 2rem;
    font-size: 12px;
    text-align: center;
}

nav a.router-link-exact-active {
    color: var(--color-text);
}

nav a.router-link-exact-active:hover {
    background-color: transparent;
}

nav a {
    display: inline-block;
    padding: 0 1rem;
    border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
    border: 0;
}

@media (min-width: 1024px) {
    header {
        display: flex;
        place-items: center;
        padding-right: calc(var(--section-gap) / 2);
    }

    .logo {
        margin: 0 2rem 0 0;
    }

    header .wrapper {
        display: flex;
        flex-wrap: wrap;
        place-items: flex-start;
    }

    nav {
        padding: 1rem 0;
        margin-top: 1rem;
        margin-left: -1rem;
        font-size: 1rem;
        text-align: left;
    }
}
</style> -->
