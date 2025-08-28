<script>
    let {message = '',type = 'success',duration = 300} = $props();
    let visible = $state(true);
    
    setTimeout(() => {
        visible = false;
    }, duration);
</script>

<div class="toast {type}" class:visible>
    {message}
</div>

<style>
    .toast {
        position: fixed;
        bottom: 20px;
        right: 20px;
        padding: 12px 24px;
        border-radius: 4px;
        color: white;
        font-weight: 500;
        opacity: 0;
        transform: translateY(100%);
        transition: all 0.3s ease-in-out;
        z-index: 1000;
    }

    .toast.visible {
        opacity: 1;
        transform: translateY(0);
    }

    .success {
        background-color: #4caf50;
    }

    .warning {
        background-color: #ff9800;
    }

    .error {
        background-color: #f44336;
    }
</style>
