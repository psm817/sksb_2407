<script>
    import {onMount} from 'svelte';
    import {goto} from '$app/navigation';

    let member = {};
    let isLogin = false;

    onMount(() => {
        // 로그인한 회원 정보 불러오기
        fetch('http://localhost:8090/api/v1/members/me', {
            credentials: 'include'
        })
            .then((response) => response.json())
            .then((data) => {
                // 회원 정보 불러오기 성공 시 데이터를 member에 담기
                if(data) {
                    console.log(data.data?.item);
                    member = data.data?.item;
                    if(data.data?.item) {
                        isLogin = true;
                        goto('/');
                    }
                }
            })
            .catch(error => {
                // 불러오기 실패 시 처리
                isLogin = false;
                console.error(error);
            });
    });

    // 로그아웃 처리
    const logout = () => {
        fetch('http://localhost:8090/api/v1/members/logout', {
            method: 'POST',
            credentials: 'include'
        })
            .then((response) => response.json())
            .then((data) => {
                // 회원 정보 불러오기 성공 시 데이터를 member에 담기
                if(data) {
                    console.log(data);
                    goto('/')
                }
            })
            .catch(error => {
                // 불러오기 실패 시 처리
                console.error(error);
            });
    };
</script>

<header>
    <ul>
        {#if isLogin}
        <li><a href="" on:click={() => logout()}>로그아웃</a></li>
        {:else}
        <li><a href="/login">로그인</a></li>
        {/if}
    </ul>
</header>

<div>
    {#if member}
        <h2>id : {member.username}</h2>
    {/if}
</div>

<slot />