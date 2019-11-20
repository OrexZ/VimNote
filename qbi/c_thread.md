
## QBI的数据结构

    >> qbi_task_data


    /*! Local QBI task data */
    struct {
    /*! Flag indicating whether we are ready to receive commands */
    boolean ready_for_cmds;

    /*! List containing commands to post to the QBI thread; list entries are
        qbi_task_cmd_s */
    qbi_util_list_s cmd_list;

    /*! Mutex protecting cmd_list */
    qbi_os_mutex_t  cmd_list_mutex;

    /*! List of registered QBI contexts. This list must only be accessed from
        the QBI task. */
    qbi_util_list_s ctx_list;

    /*! Thread control data */
    qbi_os_thread_info_t task_info;

    /*! Timer used to trigger checks for transaction timeout */
    qbi_os_timer_t timeout_timer;
    } qbi_task_data = {FALSE,};



    >> qbi_hc_linux_info


    typedef struct {
    /*! QBI context associated with this link */
    qbi_ctx_s ctx;

    /*! Indicates whether we have updated the filesystem to reflect the opened/
        closed status of the QBI context
        @see QBI_HC_LINUX_ACTIVE_SESSION_FILE */
    boolean is_session_open;

    /*! Device file descriptor used for IO with USB driver */
    int dev_fd;

    /*! Buffer used to copy data from the USB driver in read() */
    uint8 rx_buf[QBI_HC_LINUX_MAX_CTRL_XFER_SIZE];

    /*! Indicates the control packet mode as MBIM or QMUX */
    qbi_hc_linux_ctl_mode_e ctl_mode;

    /*! Tells whether DIAG is included in the active USB composition */
    qbi_hc_diag_config_e active_diag_config;

    /*! Set to TRUE when active_diag_config is populated */
    boolean active_diag_config_initialized;

    /*! Set to TRUE when DPM supported and ep_info is populated, FALSE when DPM
        not supported by the platform, UNKNOWN when EP_LOOKUP not queried yet */
    qbi_hc_linux_tristate_e dpm_supported;

    /*! Endpoint info cache lasting the duration of the MBIM session */
    hardware_accl_port_details_v01 ep_info;

    /*! Indictes whether PCIe is enabled or no. Current QBI implementation
        allows for the co-existence of PCIe with (either HSUSB or HSIC)
        provided that the PID of HSUSB or HSIC does NOT include MBIM */
    qbi_hc_linux_tristate_e  pcie_enabled;

    /*! state of mhi ctrl. The value is updated by the mhi_monitor thread */
    qbi_hc_mhi_ctrl_state_e  mhi_ctrl_state;

    /*! This will be used to monitor whether current write in progress */
    qbi_os_mutex_t mhi_monitor_write_op;

    sem_t mhi_monitor_semaphore;

    } qbi_hc_linux_info_s;



    >> ctx

    typedef struct qbi_ctx_struct {
    /*! List entry header for qbi_task's list of registered QBI contexts; must be
        first as we alias */
    qbi_util_list_entry_s list_entry;

    /*! Numeric identifier primarily used for debugging */
    qbi_ctx_id_e id;

    /*! Current state of this context, i.e. whether it has completed MBIM_OPEN,
        etc. */
    qbi_ctx_state_e state;

    /*! Maximum control transfer size, negotiated with host */
    uint32  max_xfer;

    /*! @brief Maintains state of an incoming multi-fragment command request
    */
    struct {
        /*! TRUE if a transaction is waiting on subsequent fragments */
        boolean    pending;

        /*! Number of bytes received so far */
        uint32     bytes_rcvd;

        /*! Total number of fragments expected */
        uint32     total_frag;

        /*! Sequence number of the next expected fragment */
        uint32     next_frag;

        /*! Reference to pending transaction that will be dispatched once all
            fragments are recieved */
        struct qbi_txn_struct *txn;

        /*! Timer used to ensure the time between fragments does not exceed the
            maximum allowed per MBIM spec */
        qbi_os_timer_t timer;
    } cmd_frag;

    /*! QBI transactions in progress (list contains qbi_txn_s elements) */
    qbi_util_list_s qbi_txns;

    /*! QMI transactions that are waiting for a response (list contains
        qbi_qmi_txn_s elements) */
    qbi_util_list_s pend_qmi_txns;

    /*! QBI transactions that have been queued */
    qbi_util_list_s outstanding_txns;

    /*! @brief QMI state incl. client IDs, etc.
        @see qbi_qmi_open */
    struct qbi_qmi_state_struct *qmi_state;

    /*! @brief QBI service state incl. cache, indication list, etc.
        @see qbi_svc_open */
    struct qbi_svc_state_struct *svc_state;

    /*! QBI host communications layer state pointer */
    void *hc_state;

    /*! Raw QMUX connection state */
    struct qbi_qmux_state_struct *qmux_state;

    /*! SSR struct to hold radio state and ssr in progress flag */
    qbi_qmi_ssr_info ssr_info;

    /*! Tracks if MBIM recovery is in progress */
    uint32 mbim_recovery;

    /*! Keeps track of close and open txn ids */
    uint32  txn_id_close;
    uint32  txn_id_open;

    } qbi_ctx_s;
