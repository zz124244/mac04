<script setup lang="ts">
import {ref} from 'vue'
import {activityText} from './activity-text'
import {taskText} from './task-text'
import {teamText} from './team-text'
import {transferText} from './transfer-text'
import UiIcon from './UiIcon.vue'
import {type Locale} from './i18n'
import LanguagePicker from './LanguagePicker.vue'
import './reference-profile.css'
import {profileText,type ProfileKey} from './profile-text'
import {maskRegisteredPhone} from './phone-mask'
export type WalletSummary={available:string;withdrawable:string;invested:string;frozen:string;total_assets:string;points:string;recharge_total?:string;withdrawal_net_total?:string;investment_historical_total?:string;investment_outstanding_total?:string}
const props=defineProps<{uid:string;registeredPhone:string;vipLevel:number;wallet:WalletSummary|null;locale:Locale;busy:boolean;error:string}>()
const emit=defineEmits<{navigate:[target:string];logout:[];locale:[locale:Locale];reload:[]}>()
const hidden=ref(false),notice=ref('')
const t=(key:ProfileKey)=>profileText(props.locale,key)
function amount(value:string|undefined){return hidden.value?'••••••':value===undefined?'—':'$'+value}
function open(target:string){if(['home','community','deposit','deposits','support','invest','orders','rollover','withdraw','withdrawalHistory','identity','transfer','ledger','team','invite','password','withdrawalAddress','tasks','checkin','lottery','mall','address','terms'].includes(target))emit('navigate',target);else notice.value=t('notReady')}
</script>
<template><div class="profile-page"><header class="relative z-10 bg-transparent profile-header"><div class="pt-safe px-margin"><div class="h-14 flex items-center justify-between"><div class="w-10"></div><h1 class="font-headline-sm text-headline-sm text-slate-900 font-bold absolute left-1/2 -translate-x-1/2">我的</h1><LanguagePicker :locale="locale" @change="emit('locale',$event)" /></div></div></header><main class="flex flex-col relative w-full pb-24 bg-[#f8fafc] min-h-screen"><div class="flex flex-col w-full px-margin pb-6 gap-space-sm pt-2"><p class="error" role="alert" v-if="error">{{error}} <button :disabled="busy" @click="emit('reload')" class="plain">{{t('retry')}}</button></p><p class="profile-notice" role="status" v-if="notice">{{notice}}</p>
<div class="relative overflow-hidden rounded-xl bg-white p-space-lg border border-slate-200/80 shadow-[0_2px_12px_rgba(15,23,42,0.04)]">
<div class="absolute -top-12 -right-12 w-44 h-44 rounded-full bg-emerald-500/5 blur-3xl pointer-events-none"></div>
<div class="relative z-10 flex flex-col gap-space-md">
<div class="flex items-center justify-between">
<div class="flex items-center gap-space-md min-w-0">
<div class="relative shrink-0">
<img alt="平台LOGO" class="w-14 h-14 rounded-full border-2 border-white shadow-md object-cover bg-white" src="/brand.png"/>
<div class="absolute -bottom-1 -right-1 flex items-center justify-center w-5 h-5 rounded-full bg-amber-600 text-white shadow-sm ring-2 ring-white"><span class="material-symbols-outlined text-[13px]" style='font-variation-settings: "FILL" 1;'></span></div>
</div>
<div class="flex flex-col min-w-0">
<div class="flex items-center gap-space-xs min-w-0 whitespace-nowrap"><span class="font-body-sm text-body-sm text-slate-500 whitespace-nowrap">注册号码：{{registeredPhone?maskRegisteredPhone(registeredPhone):'—'}}</span></div>
<div class="flex items-center gap-space-xs mt-0.5 min-w-0 flex-nowrap whitespace-nowrap">
<span class="font-headline-sm text-headline-sm text-slate-900 font-bold tracking-tight profile-uid whitespace-nowrap">UID:{{uid}}</span>
<span class="font-body-sm text-body-sm text-primary whitespace-nowrap ml-space-sm">{{vipLevel>0?`VIP ${vipLevel}`:'普通会员'}}</span>
</div>
</div>
</div>
</div>
</div>
</div>
<div class="relative overflow-hidden rounded-xl bg-white p-space-lg border border-slate-200/80 shadow-[0_2px_12px_rgba(15,23,42,0.04)]">
<div class="absolute top-0 right-0 w-36 h-36 bg-sky-500/5 rounded-full blur-2xl pointer-events-none"></div>
<div class="grid grid-cols-12 gap-space-md items-center"><div class="col-span-7 flex flex-col"><div class="flex items-center gap-space-xs text-slate-500"><span class="font-label-caps text-label-caps uppercase tracking-wider font-semibold">{{t('total')}} (USDT)</span><button :aria-label="t('visibility')" :aria-pressed="hidden" @click="hidden=!hidden" class="text-slate-400 hover:text-slate-700 flex items-center reference-hit-target" id="toggle-asset-visibility"><UiIcon :name="hidden?'eyeOff':'eye'" :size="16" class="text-[16px]"></UiIcon></button></div><div class="mt-1 flex items-baseline gap-1"><span class="font-label-numeric-lg text-label-numeric-lg font-bold text-slate-900" id="asset-amount">{{amount(wallet?.total_assets)}}</span></div><div class="mt-2 pt-2 border-t border-slate-100 flex flex-col"><span class="font-label-caps text-label-caps uppercase tracking-wider font-semibold text-slate-500">{{t('available')}} (USDT)</span><span class="font-label-numeric-md text-label-numeric-md font-bold text-amber-600 mt-0.5">{{amount(wallet?.available)}}</span></div></div><button @click="open('mall')" class="col-span-5 flex flex-col pl-space-md border-l border-slate-100 text-left"><div class="flex items-center justify-between text-slate-500"><span class="font-label-caps text-label-caps uppercase tracking-wider font-semibold">{{t('points')}}</span><UiIcon :size="14" class="text-[14px] text-emerald-600" name="points"></UiIcon></div><div class="mt-1"><span class="font-label-numeric-lg text-label-numeric-lg font-bold text-emerald-700">{{hidden?'••••••':wallet?.points??'—'}}</span></div><span class="font-label-caps text-label-caps text-slate-400 mt-2 block tracking-tight font-medium">{{t('pointHint')}}</span></button></div>
<div class="mt-space-lg pt-space-md border-t border-slate-100 grid grid-cols-2 gap-space-sm">
<button @click="open('deposit')" class="flex items-center justify-center gap-space-xs py-2.5 px-3 rounded-lg bg-amber-600 text-white font-headline-sm text-body-md font-semibold hover:bg-amber-700 active:opacity-90 transition-all shadow-sm"><UiIcon :size="18" class="text-[18px]" name="plus"></UiIcon><span class="">{{t('deposit')}}</span></button>
</div>
</div>
<div class="flex flex-col gap-space-xs">
<div class="flex items-center justify-between px-1">
<span class="font-label-caps text-label-caps uppercase tracking-widest text-slate-500 font-semibold">{{t('matrix')}}</span>
</div>
<div class="bg-white rounded-xl p-space-md flex flex-col gap-space-md border border-slate-200/80 shadow-[0_2px_8px_rgba(15,23,42,0.03)]">
<div class="grid grid-cols-3 gap-space-sm">
<div class="flex flex-col bg-slate-50 p-space-sm rounded-lg border border-slate-100">
<span class="font-label-caps text-label-caps text-slate-500 truncate font-semibold">{{t('recharge')}}</span>
<span class="font-label-numeric-md text-label-numeric-md font-bold text-slate-900 mt-1 truncate">{{amount(wallet?.recharge_total)}}</span>
</div>
<div class="flex flex-col bg-slate-50 p-space-sm rounded-lg border border-slate-100">
<span class="font-label-caps text-label-caps text-slate-500 truncate font-semibold">{{t('withdrawals')}}</span>
<span class="font-label-numeric-md text-label-numeric-md font-bold text-slate-900 mt-1 truncate">{{amount(wallet?.withdrawal_net_total)}}</span>
</div>
<div class="flex flex-col bg-slate-50 p-space-sm rounded-lg border border-slate-100">
<span class="font-label-caps text-label-caps text-slate-500 truncate font-semibold">{{t('investment')}}</span>
<span class="font-label-numeric-md text-label-numeric-md font-bold text-slate-900 mt-1 truncate">{{amount(wallet?.investment_historical_total)}}</span>
</div>
</div>
<div class="grid grid-cols-2 gap-space-sm">
<div class="flex flex-col justify-between bg-slate-50 p-space-md rounded-lg border border-slate-100"><div><div class="flex items-center justify-between"><span class="font-body-sm text-body-sm text-slate-600 font-medium">{{t('withdrawable')}}</span><span class="w-1.5 h-1.5 rounded-full bg-amber-600"></span></div><span class="font-label-numeric-md text-label-numeric-md font-bold text-amber-600 block mt-1">{{amount(wallet?.withdrawable)}}</span></div><button @click="open('withdraw')" class="mt-2 w-full py-1 rounded bg-amber-50 text-amber-700 border border-amber-100 font-label-caps text-label-caps font-semibold flex items-center justify-center hover:bg-amber-100 transition-colors reference-balance-action">{{t('withdraw')}}</button><button @click="open('rollover')" class="mt-2 w-full py-1 rounded bg-amber-50 text-amber-700 border border-amber-100 font-label-caps text-label-caps font-semibold flex items-center justify-center hover:bg-amber-100 transition-colors reference-balance-action">{{t('rollover')}}</button></div>
<div class="flex flex-col justify-between bg-slate-50 p-space-md rounded-lg border border-slate-100">
<div>
<div class="flex items-center justify-between">
<span class="font-body-sm text-body-sm text-slate-600 font-medium">{{t('outstanding')}}</span>
<span class="w-1.5 h-1.5 rounded-full bg-sky-600"></span>
</div>
<span class="font-label-numeric-md text-label-numeric-md font-bold text-slate-900 block mt-1">{{amount(wallet?.investment_outstanding_total)}}</span>
</div>
</div>
</div>
</div>
</div>
<div class="flex flex-col gap-space-xs">
</div>
<div class="flex flex-col gap-space-xs">
<span class="font-label-caps text-label-caps uppercase tracking-widest text-slate-500 font-semibold px-1">{{t('finance')}}</span>
<div class="bg-white rounded-xl overflow-hidden border border-slate-200/80 shadow-[0_2px_8px_rgba(15,23,42,0.03)]"><button @click="open('orders')" class="flex items-center justify-between p-space-md hover:bg-slate-50 transition-colors w-full text-left" type="button"><div class="flex items-center gap-space-md"><div class="w-8 h-8 rounded-lg bg-emerald-50 text-emerald-700 border border-emerald-100 flex items-center justify-center"><UiIcon :size="18" class="text-[18px]" name="orders"></UiIcon></div><span class="font-body-md text-body-md text-slate-900 font-medium">{{t('orders')}}</span></div><div class="flex items-center gap-space-xs text-slate-400"><UiIcon :size="18" class="text-[18px]" name="next"></UiIcon></div></button><button @click="open('team')" class="flex items-center justify-between p-space-md hover:bg-slate-50 transition-colors border-t border-slate-100 w-full text-left" type="button"><div class="flex items-center gap-space-md"><div class="w-8 h-8 rounded-lg bg-sky-50 text-sky-700 border border-sky-100 flex items-center justify-center"><UiIcon :size="18" class="text-[18px]" name="team"></UiIcon></div><span class="font-body-md text-body-md text-slate-900 font-medium">{{t('team')}}</span></div><div class="flex items-center gap-space-xs text-slate-400"><UiIcon :size="18" class="text-[18px]" name="next"></UiIcon></div></button><button @click="open('deposits')" class="flex items-center justify-between p-space-md hover:bg-slate-50 transition-colors border-t border-slate-100 w-full text-left" type="button"><div class="flex items-center gap-space-md"><div class="w-8 h-8 rounded-lg bg-emerald-50 text-emerald-700 border border-emerald-100 flex items-center justify-center"><UiIcon :size="18" class="text-[18px]" name="down"></UiIcon></div><span class="font-body-md text-body-md text-slate-900 font-medium">{{t('deposits')}}</span></div><div class="flex items-center gap-space-xs text-slate-400"><UiIcon :size="18" class="text-[18px]" name="next"></UiIcon></div></button><button @click="open('withdrawalHistory')" class="flex items-center justify-between p-space-md hover:bg-slate-50 transition-colors border-t border-slate-100 w-full text-left" type="button"><div class="flex items-center gap-space-md"><div class="w-8 h-8 rounded-lg bg-sky-50 text-sky-700 border border-sky-100 flex items-center justify-center"><UiIcon :size="18" class="text-[18px]" name="up"></UiIcon></div><span class="font-body-md text-body-md text-slate-900 font-medium">{{t('withdrawalHistory')}}</span></div><div class="flex items-center gap-space-xs text-slate-400"><UiIcon :size="18" class="text-[18px]" name="next"></UiIcon></div></button><button @click="open('ledger')" class="flex items-center justify-between p-space-md hover:bg-slate-50 transition-colors border-t border-slate-100 w-full text-left" type="button"><div class="flex items-center gap-space-md"><div class="w-8 h-8 rounded-lg bg-emerald-50 text-emerald-700 border border-emerald-100 flex items-center justify-center"><UiIcon :size="18" class="text-[18px]" name="ledger"></UiIcon></div><span class="font-body-md text-body-md text-slate-900 font-medium">{{t('ledger')}}</span></div><div class="flex items-center gap-space-xs text-slate-400"><UiIcon :size="18" class="text-[18px]" name="next"></UiIcon></div></button></div>
</div>
<div class="flex flex-col gap-space-xs">
<span class="font-label-caps text-label-caps uppercase tracking-widest text-slate-500 font-semibold px-1">{{t('security')}}</span>
<div class="bg-white rounded-xl overflow-hidden border border-slate-200/80 shadow-[0_2px_8px_rgba(15,23,42,0.03)]">
<button @click="open('identity')" class="flex items-center justify-between p-space-md hover:bg-slate-50 transition-colors w-full text-left" type="button">
<div class="flex items-center gap-space-md">
<div class="w-8 h-8 rounded-lg bg-emerald-50 text-emerald-700 border border-emerald-100 flex items-center justify-center">
<UiIcon :size="18" class="text-[18px]" name="identity"></UiIcon>
</div>
<span class="font-body-md text-body-md text-slate-900 font-medium">{{t('identity')}}</span>
</div>
<div class="flex items-center gap-space-xs">
<UiIcon :size="18" class="text-[18px] text-slate-400" name="next"></UiIcon>
</div>
</button>
<button @click="open('address')" class="flex items-center justify-between p-space-md hover:bg-slate-50 transition-colors border-t border-slate-100 w-full text-left" type="button">
<div class="flex items-center gap-space-md">
<div class="w-8 h-8 rounded-lg bg-slate-100 text-slate-600 flex items-center justify-center">
<UiIcon :size="18" class="text-[18px]" name="address"></UiIcon>
</div>
<span class="font-body-md text-body-md text-slate-900 font-medium">{{t('address')}}</span>
</div>
<div class="flex items-center gap-space-xs text-slate-400">
<UiIcon :size="18" class="text-[18px]" name="next"></UiIcon>
</div>
</button>
<button @click="open('terms')" class="flex items-center justify-between p-space-md hover:bg-slate-50 transition-colors border-t border-slate-100 w-full text-left" type="button">
<div class="flex items-center gap-space-md">
<div class="w-8 h-8 rounded-lg bg-slate-100 text-slate-600 flex items-center justify-center">
<UiIcon :size="18" class="text-[18px]" name="terms"></UiIcon>
</div>
<span class="font-body-md text-body-md text-slate-900 font-medium">{{t('terms')}}</span>
</div>
<div class="flex items-center gap-space-xs text-slate-400">
<UiIcon :size="18" class="text-[18px]" name="next"></UiIcon>
</div>
</button>
<button @click="open('password')" class="flex items-center justify-between p-space-md hover:bg-slate-50 transition-colors border-t border-slate-100 w-full text-left" type="button">
<div class="flex items-center gap-space-md">
<div class="w-8 h-8 rounded-lg bg-slate-100 text-slate-600 flex items-center justify-center">
<UiIcon :size="18" class="text-[18px]" name="password"></UiIcon>
</div>
<span class="font-body-md text-body-md text-slate-900 font-medium">{{t('password')}}</span>
</div>
<div class="flex items-center gap-space-xs text-slate-400">
<UiIcon :size="18" class="text-[18px]" name="next"></UiIcon>
</div>
</button>
<button @click="open('withdrawalAddress')" class="flex items-center justify-between p-space-md hover:bg-slate-50 transition-colors border-t border-slate-100 w-full text-left" type="button">
<div class="flex items-center gap-space-md">
<div class="w-8 h-8 rounded-lg bg-slate-100 text-slate-600 flex items-center justify-center">
<UiIcon :size="18" class="text-[18px]" name="account_balance_wallet"></UiIcon>
</div>
<span class="font-body-md text-body-md text-slate-900 font-medium">{{t('withdrawalAddress')}}</span>
</div>
<div class="flex items-center gap-space-xs text-slate-400">
<UiIcon :size="18" class="text-[18px]" name="next"></UiIcon>
</div>
</button>
</div>
</div>
<div class="pt-space-xs">
<button :disabled="busy" @click="emit('logout')" class="w-full py-3.5 px-4 rounded-xl bg-white border border-rose-200 text-rose-600 hover:bg-rose-50 active:bg-rose-100 transition-colors flex items-center justify-center gap-space-xs font-body-md font-semibold shadow-sm" id="btn-logout">
<UiIcon :size="20" class="text-[20px]" name="logout"></UiIcon>
<span class="">{{t('logout')}}</span>
</button>
</div>
</div>
</main><nav class="fixed bottom-0 inset-x-0 z-50 pb-safe bg-white/95 backdrop-blur-xl border-t border-slate-200/90 shadow-[0_-4px_16px_rgba(0,0,0,0.06)] profile-navigation" data-active-classes="text-emerald-700 [&amp;_.nav-dot]:opacity-100 [&amp;_.nav-glow]:opacity-100"><div class="flex justify-around items-center h-16 px-space-xs max-w-md mx-auto"><button @click="open('home')" class="group flex flex-col items-center justify-center relative w-16 h-12 text-slate-500 transition-colors hover:text-slate-900" type="button"><UiIcon :size="24" class="text-[24px] relative z-10" name="explore"></UiIcon><span class="font-label-caps text-label-caps mt-1 relative z-10 font-semibold">{{t('home')}}</span><span class="nav-dot w-1 h-1 rounded-full bg-emerald-600 mt-0.5 opacity-0 transition-opacity"></span><span class="nav-glow absolute inset-0 rounded-xl bg-amber-500/10 opacity-0 transition-opacity pointer-events-none"></span></button><button @click="open('invest')" class="group flex flex-col items-center justify-center relative w-16 h-12 text-slate-500 transition-colors hover:text-slate-900" type="button"><UiIcon :size="24" class="text-[24px] relative z-10" name="trending_up"></UiIcon><span class="font-label-caps text-label-caps mt-1 relative z-10 font-semibold">{{t('invest')}}</span><span class="nav-dot w-1 h-1 rounded-full bg-emerald-600 mt-0.5 opacity-0 transition-opacity"></span><span class="nav-glow absolute inset-0 rounded-xl bg-amber-500/10 opacity-0 transition-opacity pointer-events-none"></span></button><button @click="open('community')" class="group flex flex-col items-center justify-center relative w-16 h-12 text-slate-500 transition-colors hover:text-slate-900" type="button"><UiIcon :size="24" class="text-[24px] relative z-10" name="groups"></UiIcon><span class="font-label-caps text-label-caps mt-1 relative z-10 font-semibold">{{t('community')}}</span><span class="nav-dot w-1 h-1 rounded-full bg-emerald-600 mt-0.5 opacity-0 transition-opacity"></span><span class="nav-glow absolute inset-0 rounded-xl bg-amber-500/10 opacity-0 transition-opacity pointer-events-none"></span></button><button @click="" aria-current="page" class="group flex flex-col items-center justify-center relative w-16 h-12 transition-colors text-amber-600 [&amp;_.nav-dot]:opacity-100 [&amp;_.nav-glow]:opacity-100" type="button"><UiIcon :size="24" class="text-[24px] relative z-10" name="account_balance_wallet"></UiIcon><span class="font-label-caps text-label-caps mt-1 relative z-10 font-bold">{{t('mine')}}</span><span class="nav-dot w-1 h-1 rounded-full bg-amber-600 mt-0.5 opacity-100 transition-opacity"></span><span class="nav-glow absolute inset-0 rounded-xl bg-amber-50 opacity-100 transition-opacity pointer-events-none"></span></button></div></nav></div></template>
<style scoped>
.profile-page{max-width:480px;margin:0 auto;min-height:100vh;background:#f8fafc;color:#0f172a;font:400 14px/20px Inter,system-ui,sans-serif;letter-spacing:0}.profile-header{left:auto;right:auto;width:100%;max-width:100%;transform:none}.profile-navigation{left:50%;right:auto;width:100%;max-width:480px;transform:translateX(-50%)}.profile-header{background:#f8fafcf0;backdrop-filter:blur(12px)}.profile-page main{padding-top:8px;padding-bottom:calc(96px + env(safe-area-inset-bottom,0px));overflow-x:hidden}.profile-page .profile-uid{font:700 18px/26px Inter,system-ui,sans-serif;overflow-wrap:anywhere;min-width:0}.reference-hit-target{position:relative;flex-shrink:0}.reference-hit-target::after{content:'';position:absolute;left:50%;top:50%;width:44px;height:44px;transform:translate(-50%,-50%)}.reference-balance-action{min-height:26px}.profile-notice{position:fixed;bottom:90px;left:50%;transform:translateX(-50%);width:max-content;max-width:calc(100% - 40px);z-index:70;padding:10px 12px;color:#047857;background:#ecfdf5;border-radius:8px;box-shadow:0 4px 20px #0f172a26}.profile-page button{cursor:pointer}.profile-page strong,.profile-page span{min-width:0}.profile-page .font-label-numeric-lg,.profile-page .font-label-numeric-md{overflow-wrap:anywhere}.profile-page .pb-safe{padding-bottom:calc(env(safe-area-inset-bottom,0px) + 8px)}
</style>

