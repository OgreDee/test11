function listen_profiler(func, globalFunc)
  local ff = nil
  if globalFunc then
    ff = p_gmCtrl.GetLocal(globalFunc, func)
  else
    ff = func
  end

  if ff == nil then
    netMgr:DebugLogError("方法找不到 ...")
    return
  end

  local _time = os.clock()
  debug.sethook(function(hooktype)
    local f = debug.getinfo(2, 'nfS')
    if f.what == "Lua" and f.func ~= nil and f.func == ff then
      if hooktype == "call" then
        netMgr:DebugLogError("call ...")
        _time = os.clock()
      elseif hooktype == "return" then
        netMgr:DebugLogError("return ...")
        netMgr:DebugLogError("调用耗时： "..(os.clock() - _time))
      end
    end
  end, "cr")
end

listen_profiler(ChatSystemManager.OnMsgListResponse)
