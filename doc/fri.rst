FRI
===
This ``package`` integrates KUKA's Fast Robot Interface (FRI) into the ROS 2 ``ament_cmake`` build system. It leaves all source code untouched.

Robot Setup
-----------
.. note::
    Please also refer to :ref:`Additional Resources`.

Verify Connection to the Robot
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1. Turn on the robot (enable green power switch on the robot controller).
2. Connect your computer to the robot controller at ``X66`` (default IP: ``172.31.1.147``) via an ethernet cable.
3. Configure the same network on your computer, therefore, set your IP to ``172.31.1.148`` or similar.
4. Try ping the robot ``ping 172.31.1.147``, expect something like:

.. code-block:: bash
    
    PING 172.31.1.147 (172.31.1.147) 56(84) bytes of data.
    64 bytes from 172.31.1.147: icmp_seq=1 ttl=64 time=0.868 ms

Install Applications to the Robot
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. note::
    Windows is required for installing the applications.

1. Follow `Verify Connection to the Robot`_.
2. Install ``Sunrise Workbench`` on you computer (if you are part of King's download from :ref:`Additional Resources`).
3. Create a new project in ``Sunrise Workbench``:
   
    - .. dropdown:: ``File`` -> ``New`` -> ``Sunrise project``

        .. thumbnail:: img/computer/00_sunrise_create_project.png

    - .. dropdown:: Leave the default IP ``172.31.1.147`` and click ``Next``

        .. thumbnail:: img/computer/01_sunrise_create_project_default_ip.png

    - .. dropdown:: Name your project, e.g. ``lbr_fri_ros2`` and click ``Next``

        .. thumbnail:: img/computer/02_sunrise_create_project_project_name.png

    - .. dropdown:: Select your robot, e.g. LBR Med 7 R800 and click ``Next``

        .. thumbnail:: img/computer/03_sunrise_create_project_robot_select.png

    - .. dropdown:: Select your Media Flange and click ``Next``

        .. thumbnail:: img/computer/04_sunrise_create_project_media_flange.png

    - .. dropdown:: Unselect ``Create Sunrise application (starts another wizard)`` and press ``Finish``

        .. thumbnail:: img/computer/05_sunrise_create_project_unselect_wizard.png

4. Configure ``SafetyConfigurations.conf`` to your needs.

5. Configure ``StationSetup.cat`` and install to controller:

    - .. dropdown:: Select your robot topology

        .. thumbnail:: img/computer/00_station_setup_topology.png

    - .. dropdown:: Select FRI software and examples

        .. thumbnail:: img/computer/01_station_setup_software.png

    - .. dropdown:: Configure your network for ``X66`` and ``KONI``

        .. thumbnail:: img/computer/02_station_setup_configuration.png

    - .. dropdown:: Click ``Install`` -> ``Save and apply``

        .. thumbnail:: img/computer/03_station_setup_installation.png

    - .. dropdown:: Click ``Ok``

        .. thumbnail:: img/computer/04_station_setup_installation_install.png

    - .. dropdown:: When asked to reboot press ``OK``

        .. thumbnail:: img/computer/05_station_setup_installation_reboot.png

    - .. dropdown:: After reboot, synchronize applications

        .. thumbnail:: img/computer/06_station_setup_installation_synchronize.png

.. note::
    This procedure installs KUKA's FRI example applications ``LBRJointSineOverlay``, ``LBRTorqueSineOverlay`` and ``LBRWrenchSineOverlay`` to the controller. They can already be used with :ref:`LBR FRI ROS 2 Demos`. To use the full :ref:`LBR FRI ROS 2 Stack`, further install the ``LBRServer`` application.

6. Install the ``LBRServer`` application:

    - .. dropdown:: Right click ``src`` -> ``New`` -> ``Package``

        .. thumbnail:: img/computer/00_lbr_fri_ros2_create_package.png

    - .. dropdown:: Name your package ``lbr_fri_ros2`` and click ``Next``

        .. thumbnail:: img/computer/01_lbr_fri_ros2_create_package_name.png
            
    - .. dropdown:: Open a `Windows Terminal <https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=en-gb&gl=gb&rtc=1>`_ and clone the ``fri`` package

        .. code-block:: bash

            git clone https://github.com/KCL-BMEIS/fri.git -b ros2-fri-1.15 $HOME\Downloads\fri
            
    - .. dropdown:: Open a `Windows Terminal <https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701?hl=en-gb&gl=gb&rtc=1>`_ as ``Administrator`` and create a symbolic link to ``LBRServer.java``

        .. code-block:: bash

            New-Item -ItemType SymbolicLink -Path $HOME\SunriseWorkspace\lbr_fri_ros2\src\lbr_fri_ros2\LBRServer.java -Target $HOME\Downloads\fri\server_app\LBRServer.java

    - .. dropdown:: Refresh source in ``Sunrise Workbench``. The ``LBRServer.java`` should now show in ``src``

        .. thumbnail:: img/computer/00_link_path_refresh.png

    - .. dropdown:: Synchronize applications

        .. thumbnail:: img/computer/06_station_setup_installation_synchronize.png

.. note::
    You can now fully use the :ref:`LBR FRI ROS 2 Stack` **or** `Write your own ROS 2 Application`_.

Write your own ROS 2 Application
----------------------------------
.. note::
    If you intend to use the robot quickly, you can use the ``lbr_fri_ros2_stack`` (:ref:`LBR FRI ROS 2 Stack`), which is built on top of this package. 

If you intend to use the FRI in your own ROS 2 project, you can do so through the following:

- Add this repository to your workspace

.. code-block:: bash

    git clone https://github.com/KCL-BMEIS/fri.git -b ros2-fri-1.15

- In your ``package.xml`` add: 

.. code-block:: xml
    
    <depend>fri</depend>

- In your ``CMakeLists.txt`` add:

.. code-block:: cmake
    
    find_package(fri REQUIRED)

API
~~~
For the ``Doxygen`` generated API, checkout `fri <../../docs/doxygen/fri/html/hierarchy.html>`_.
